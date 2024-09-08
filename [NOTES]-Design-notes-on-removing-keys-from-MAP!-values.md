# Introduction

Currently, you can remove a key from a `map!` by setting it to `none`. Although this works very nicely, it has a significant drawback: if you're trying, for example, to use `map!` to represent an object imported from JSON data, then you can't distinguish between a key being there but set to `null` and a key not being there at all. (Since the most obvious mapping for JSON `null` is `none` in Red.)

In a more general sense, `map!` is the only case in Red where setting something to `none` "removes" it, and it seems arbitrary. With blocks, or maps, you can use `find` to determine if a key exists, but there is no way to have a key associated with a `none!` value in maps currently, which makes it quite different.

## Why does it currently work this way?

The reason behind using `none` to remove a key is that `map!` is not a series, and so you can't just use `remove`, and it's not a context (besides, keys are not limited to words) so you can't just use `unset`. It is also a common idiom for associative arrays in other languages. Another reason is the desire for symmetry with the main way the keys are set (using path access). [Gab: I don't really agree here, for eg. in Javascript you set a key to `null` with `obj.key = null` but remove the key with `delete obj.key`; in fact I'm not sure assignment as way to remove a key is a great idea.]

> Gregg: Agree with Gab. It's a carryover from AWK, or maybe even before that.

# Possible solutions

## Add a special case to `remove/part`

`remove` is a series action, and the `/part` refinement generally means to remove more than one value from the series. However, it would be possible to allow `map!` as an argument and also allow any value as the `/part` argument. Then one could remove a key with:

```red
remove/part map 'key
remove/part map "key"
```

...and so on. There is precedent in `bitset!`:

```red
>> b: make bitset! "ABC"
== make bitset! #{000000000000000070}
>> remove b
*** Script Error: missing a required argument or refinement
*** Where: remove
*** Stack:  

>> remove/part b #"B"
== make bitset! #{000000000000000050}
```

The downside is that this is a special case and somewhat pollutes the meaning of `/part`, in addition to making it a required refinement.

## Use `unset!` instead of `none` to remove keys

Instead of the current `map/key: none` to remove `key` from `map`, use something like `map/key: make unset! none`. It makes more sense logically, given the meaning of `unset!`, however it's annoying to ever have to explicitly deal with `unset!` values. A mezz helper function could be added though, and it could support other cases as well (including perhaps `bitset!`, so we can get rid of the `remove/part` special case altogether).

This is a bit inconsistent with words, which require `set/any` to accept `unset!`.

## Allow `unset` (the `native!` function) on `path!` values and handle all cases accordingly

A similar but perhaps cleaner alternative to the above would be to allow `unset 'map/key` to remove `key` from `map`. It gets a little ugly with `string!` keys etc, you'd have to use `unset 'map/("key")`. It would be perhaps more familiar to people coming from Javascript for eg. (`delete object.key` becomes `unset 'object/key`).

There is a precedent for this with `get` and `set`, which both allow a map path. It does start feeling a little strange, but as an underlying mechanism, with a wrapper mezz, it might be the most consistent. Unless `remove/part` to match bitset behavior is deemed better. `Unset` is shorter. And also doesn't preclude using `unset!` as a value to remove a key.

## Add a new action, `remove-key`

It feels a bit "ugly" to add an action that only serves one type, however, it would be handy on all series values (just like `append` is unnecessary but handy to have), and would solve the problem for `map!` (and, arguably, `bitset!`). It might be useful in the future for other types as well.

## Turn `map!` into a proper series

Given that we seem to care about the order of keys in `map!` (as well as `object!`), at least up to some extent, it might not be so outrageous to allow `map!` values to have a current position as well, and let most series actions work on `map!`, including `remove`. This definitely needs more thought, however, I think it would be a big win if we could figure out how to leverage the series abstraction here.

## And the winner is...

A new idea, not listed above, but close to others. We'll add a `/key` refinement to `remove`, which will be used for both maps and bitsets. The bitset code will change because of that, but it means `/part` won't be overloaded for this purpose. 

While this seems like the best choice, it won't come particularly cheap. `Remove` is an action, and therefore implemented for each type independently. Each instance must be updated to support the new refinement and its argument, even if only to return an error saying they don't support that refinement. Interaction with `/part` must also be considered. It seems best to make `/part` and `/key` mutually exclusive. However, if they're mutually exclusive, we could argue again for `remove/part`.

The new refinement could also be used for series values, replacing the `remove-key` idea above. And if we have `extend`, it could be the opposite of that for objects (though the naming isn't great for that).

## P.S. `remove find map key`

Chat is here: :point_up: [January 21, 2021 10:02 PM](https://rebol.tech/gitter.im/red/red/2021/#msg6009cfc836db01248a9265bb)<br>
General idea is: each key in the map does have an index, even if it is internal. What happens inside `remove/key` is that it finds that index, and removes the key. We could just pack that index (chosen key) with the map cell, and `remove find map key` becomes reality.<br>
Another idea is: maps do have order (`foreach .. map` is just one proof). Even if this order is not guaranteed (in some future implementation) to persist between map modifications, we still can use key as the index and use half of the series functions to work with maps.<br>
Another idea (by Gabriele) is: let maps have order, implement it as a linked list. And all series functions will work on maps (I imagine only keys array would be exposed to series functions, while values remain a domain of put and select). 

## Usage Notes

### Blocks as keys

```
>> blk: [[a b] 1 [b c] 2]
== [[a b] 1 [b c] 2]
>> remove/key blk [[a b]]
== [[b c] 2]
```

It's a bit awkward, because it uses find rather than find/only internally for series. It should probably use find/only as it doesn't take the block length into account, and that's a bug IMO.

```
>> blk: [ a b 1  b c 2]
== [a b 1 b c 2]
>> remove/key blk [a b]
== [1 b c 2]
```

You can't currently create maps with blocks as keys, so it should probably throw an error with your call @semseddin.

The design was never to have it remove multiple keys in a block, which would make it impossible to use for block keys, should maps support them in the future.
