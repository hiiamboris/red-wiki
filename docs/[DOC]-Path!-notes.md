`Path!` values are special in many ways. They are a block type, but don't require any bracketing characters to enclose them. Instead, they use the `/` character as an element separator. This is natural, and applied widely in Red. When a path is evaluated, as when accessing nested elements in structures, or when calling functions, the type of value in each slot (sometimes called a _segment_ when discussing paths) controls evaluation. That is, words, integers, get-words, and parens can be used to control literal and parameterized values in path segments.

But what happens in other cases? Red has a _lot_ of datatypes. How does each of them behave when used in a `path!`? 

## `path!` values in slots

Can you have a path within a path? Yes, you can. Look at this example:

```red
>> p1: to path! [a b c]
== a/b/c
>> p2: to path! [a b/c]
== a/b/c
>> p1 = p2
== false
>> mold p1
== "a/b/c"
>> mold p2
== "a/b/c"
>> length? p1
== 3
>> length? p2
== 2
>> p1/2
== b
>> p2/2
== b/c
>> to block! p1
== [a b c]
>> to block! p2
== [a b/c]

>> to path! reduce ['b to block! 'name/phone]
== b/[name phone]
```

The two path values look the same when rendered, but are not the same internally. We have the same behavior in many other datatypes in Red:

```red
>> word! = first [word!]
== false
>> none = first [none]
== false
```

The lexical space used by Red is not big enough to represent all the possible values you can construct at runtime (even construction syntax cannot represent everything, like different bindings for the same word), so we can end up with overlapping text representations. That's a fact of life in Red's design. We can address it in various ways (e.g., syntax highlighting), but it's important to know that it exists. 

# Paths in paths?

> Paths within paths within paths? Shoot me if there's any use for this except spawning more bugs.

Paths are block-like datatype, differentiated only by their literal form. Whatever use you can have for a data structure containing nested blocks, or nested parens can be applied to nested paths. They surely don't read nicely when printed, but that doesn't mean that having an extra datatype for block-like values is not useful. Moreover, trying to "remove" such constructions from the language would only result in increasing the complexity of the codebase, slowing down the performance, and introducing an arbitrary exception/quirk in the language semantics, for no practical gain for end users. "Less is more".

Here is a value: `[a [b c]]`. That block value is fully equivalent to `a/b/c` where `b/c` is a sub-path. Internally, they are exactly the same and differ only by their type ID. The fact that the syntactic representation is not unique, is a representation limitation that will be addressed. It has no bearing on the safety of the language more than any other series type from the `any-block!` typeset.

> So I think now I see the reasons why it's done like this, at least the tip of the iceberg. It looks like though the internal representation of paths and words is way less restrictive than the syntax of the language, which has its benefits (like I can make an empty path and build upon it), but also leads to some confusion (as to what is valid and what isn't).

The semantics of the Red and Rebol languages allow the construction of many values that don't have a unique syntactic form, or don't have a syntactic form at all. Despite that, such values are legal, because they are simply the result of legal semantics. Blocking some of those values (one would yet have to define a viable/reliable way to achieve that), would introduce exceptions in the semantics, breaking their regularity, predictability and simplicity.

Though this is not entirely satisfying, because some values can be easily and uniquely visually represented, and other cannot (or at least not by the default formatting output methods). The culprit here is not the language semantics, it's the syntactic representations, or rather the limitations caused by our restricted set of readable symbols that we can use to create human-friendly and meaningful literal forms.

The "cure" does not lie in crippling the language and datatypes semantics, but in providing better visualisations for the whole spectrum of values that can be produced at run-time. Some options:

`mold/all`: provides a so-called "construction syntax" capable of representing many values which don't have a proper literal form. It is mostly useful for I/O-oriented serialization needs, as it's not very elegant for humans to read/write. Example (in Rebol, not yet implemented in Red): `>> mold/all next 'a/b == "#[path! [a b] 2]"`

`mold/bin`: provides a serialization format capable of representing all possible values, retaining all their properties, including bindings, contexts and circular references. The resulting format is purely binary, so not human-readable, but that's the price to pay for a bijective representation of all the possible values. It's called Redbin format (exclusively in Red), and only the decoder is currently implemented in Red's runtime.

Syntax coloring in Red console: we are experimenting with datatype-based syntax coloring output in the new 0.6.4 console engine.

Syntax coloring in an IDE: only static coloring is available for now (in Red's VSCode plugin), a live coloring would need an IDE deeper integrated with Red's runtime.

Come up with more first-class literal forms: the syntactic space of human-friendly and readable forms is pretty well occupied by Red forms already. We have a few branches that can be used (like for a unit! datatype), but not many. So I don't see this as a long-term solution for covering all the needs.

# Confusion about words and paths with only one value in them

See: https://www.reddit.com/r/redlang/comments/86kdwr/on_words_vs_paths_confusion/

The cause of some confusion is that users might have missed that words are atomic values while paths are containers (more precisely series), like blocks, that's why path types are part of `any-block!` typeset:
```red
>> any-block!
== make typeset! [block! paren! path! lit-path! set-path! get-path! hash!]
```
Moreover, paths can contain different kinds of values, not just words (though they do require a word as the first element):
```red
>> 'a/1/("hello")
== a/1/("hello")
```
Given those facts, an equivalence between words and paths would make no sense, because their nature is very different.

> While it also seems easy to introduce a set of features that will fix it all: `make to-path, to-set-path and to-get-path accept word!, get-word!, set-word!`

This is already a feature of the language, but you'll notice that it's not bijective, as an atomic value can be converted to a container with that atomic value as its single element (basically, it's a wrapping operation), though the converse, converting a series with any number of values to an atomic value makes no sense.

If we restrict the series to only series of single element, would that make sense to allow conversion, let's say from a "singular path" to a word? It would make sense, though it doesn't need to be implemented, because it's already an existing feature: simply extracting a value from a series. For example:
```red
>> p: to-path 'a
== a
>> type? p
== path!
>> type? probe first p
a
== word!
```
You can use `first` or `pick` to get your word from the path, so the feature is already covered with basic series semantics.

So far, so good, right? Well, not exactly. What you've called "singular path" is ill-defined. Let's say you define it as a path where the following test would return `true: 1 = length? path`. Let's now see some examples:
```red
>> p: 'a/b
== a/b
>> 1 = length? p
== false
>> q: next p
== b
>> 1 = length? q
== true
>> length? head q
== 2
```
As you can see, it's not that simple, because paths are series, they have an implicit offset position. So `p` is a path of length 2 (not singular), while `q` is a path of length 1 (singular). But `q` is actually referring to a path of length 2 when the offset is at is head. `q` refers to the same underlying series as `p` differing only in the offset position:
```red
>> poke p 2 123
== 123
>> p
== a/123
>> q
== 123
>> 1 = length? q
== true
>> insert p 'new
== a/123
>> 1 = length? q
== false
```

Making an equivalence between a "singular path" and a word value is not something that would be natural in many use-cases. So we have to restrict the definition of "singular path" to the paths where `1 = length? head path` returns true. This kind of path is actually a rare occurence in real code, and usually a temporary state while building a path of length > 1.

> Honestly, I can live with it, and just wrap the whole thing into my own comparison and conversion functions, or convert words to paths when they appear and forget that they were ever there. No big deal.

That would be a waste of resources (converting atomic value to lists) and deliberately reducing the richness of the language.

# Compared to blocks

A block is a sequence of values with an implicit position (a series). Block's literal form supports any literal value. Block's syntax relies on starting/ending delimiters. A path is a sequence of values with an implicit position (a series). Paths have a restricted literal form compared to blocks, supporting only a subset of literal values and requiring a starting word. Path syntax relies on separators between values.

It's a relative thing (the "R" in Rebol). In the main language, a block is the general data structure for holding values. A path is used to describe a hierarchical access in a value (series, objects, maps, tuples, pairs, etc...) with different possible tail semantics (pick, select, get, poke, etc...), or to represent a function call with refinements. In a dialect, a block or a path could mean something different, depending on the dialect's semantics. Each dialect could have a different meaning for those datatypes.

# Finding and Selecting on paths

Because paths are block values, you need to remember that `/only` should be used with functions that support it, if you want to match nested paths as single values.

```red
>> b: [1 2 3 a b c/d e/f g/h]
== [1 2 3 a b c/d e/f g/h]
>> find/only b 'c/d
== [c/d e/f g/h]
>> select/only b 'c/d
== e/f
>> h: make hash! b
== make hash! [1 2 3 a b c/d e/f g/h]
>> select/only h 'c/d
== e/f
```

And that holds true even if you construct convoluted paths programmatically (we won't recommend this, but it works).

```red
>> p: to path! [a b/c [d e/f [g]]]
== a/b/c/[d e/f [g]]
>> sp: select/only p 'b/c
== [d e/f [g]]
>> ssp: select/only sp 'e/f
== [g]
```

Just because you *can* create `path!` values like this doesn't mean you can round trip them, as you may create a path that can't be represented in direct lexical form.