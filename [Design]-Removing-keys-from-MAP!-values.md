# Introduction

Currently, you can remove a key from a `map!` by setting it to `none`. Although this works very nicely, it has a significant drawback: if you're trying, for example, to use `map!` to represent an object imported from JSON data, then you can't distinguish between a key being there but set to `null` and a key not being there at all. (Since the most obvious mapping for JSON `null` is `none` in Red.)

In a more general sense, `map!` is the only case in Red where setting something to `none` "removes" it, and it seems arbitrary.

## Why does it currently work this way?

The reason behind using `none` to remove a key is that `map!` is not a series, and so you can't just use `remove`, and it's not a context (besides, keys are not limited to words) so you can't just use `unset`. It is also a common idiom for associative arrays in other languages.

# Possible solutions

## Add a special case to `remove/part`

`remove` is a series action, and the `/part` refinement generally means to remove more than one value from the series. However, it would be possible to allow `map!` as an argument and also allow any value as the `/part` argument. Then one could remove a key with:

```
remove/part map 'key
remove/part map "key"
```

...and so on. There is precedent in `bitset!`:

```
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

## Allow `unset` (the `native!` function) on `path!` values and handle all cases accordingly

A similar but perhaps cleaner alternative to the above would be to allow `unset 'map/key` to remove `key` from `map`. It gets a little ugly with `string!` keys etc, you'd have to use `unset 'map/("key")`. It would be perhaps more familiar to people coming from Javascript for eg. (`delete object.key` becomes `unset 'object/key`).