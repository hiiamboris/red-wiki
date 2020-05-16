### Introduction

Red provides a flexible set of aggregate structures, each coming with its own trade-offs. Said trade-offs are not entirely obvious to beginners and newcomers, who frequently ask about the difference between `vector!` and `block!`, `block!` and `hash!`, `hash!` and `map!`, `map!` and `object!`.

This page addresses the respective pros, cons and common use-cases of these datatypes.

### Table of contents

1. [Phrasebook](#phrasebook)
1. [`block!`](#block)
1. [`vector!`](#vector)
1. [`object!`](#object)
1. [`hash!`](#hash)
1. [`map!`](#map)
1. [Algorithmic complexity](#algorithmic-complexity)

### Phrasebook

- *Indexing operations* — `at`, `pick`, `poke` and the like, path accessors with `integer!` keys;
- *Key/value queries* — `select`, `put`, and path accessors with non-`integer!` keys;
- *Hashable* — `scalar! all-word! any-string!`.

### `block!`

Blocks are bread and butter of Red programming used in all facets of coding: from daily scripting, records and data structures to metaprogramming and dialects.

Blocks are completely free-form and can hold values of any type, providing a convenient `series!` interface for their access.

Series are arrays by nature, and search in them takes linear time, so as key/value queries; however, indexing operations take constant time, due to `series!`' internal optimizations.

Use `block!` whenever you need a general-purpose container that you frequently update, shrink, expand and pass around between functions.

### `vector!`

Vectors are homogeneous series (akin to `string!`) that can hold only one of a limited set of datatypes. They support most of the scalar and bitwise actions, while providing precise control over the element's size.

As such, the main advantages of `vector!` are convenient math operations and memory savings (compared to `any-block!` values, which hold cells 4 machine pointers in size). Datatypes supported by `vector!` and their sizes are shown in the table below:

| Datatype   | Sizes (bits) |
|:-----------|:------------:|
| `integer!` | 8 16 32      |
| `char!`    | 8 16 32      |
| `float!`   | 32 64        |
| `percent!` | 64           |

It should be noted that `vector!` does not catch math overflow errors, and all math / bitwise actions are performed in place (i.e. they mutate vector) and element-wise (i.e. in a linear fashion).

Use `vector!` for numerical tasks or to effectively store groups of scalar values.

### `object!`

Just like blocks, objects form the backbone of the language. Red's object model is influenced by prototype-based and reactive programming, without strictly adhering to an Object-Oriented Paradigm. In this way, objects cannot form inheritance chains and act only as namespaces, grouping related values together and following information hiding principle.

Objects provide a key/value interface and cannot be indexed with `series!` actions; its keys are strictly limited to `set-word!` datatype, and accessing its values via bounded words takes constant time; binding words to object's context or adding new words is also a constant time operation due to internal usage of a hashtable.

Once created, objects cannot be extended with new entries (*this may change in the future*), nor can existing entries be deleted; however, an object can be used as a prototype for another object.

Use `object!` to group functionally-related data together and encapsulate your code's logic. Don't follow classic OOP principles too strongly; instead, seek to leverage `object!`s in idiomatic ways: by using reactors, bindings and reflection with ownership system.

### `hash!`

Hashes are ordered series, and can be queried and indexed just like blocks, without imposing any limitations on types of stored values.

Unlike blocks, hashes have much faster lookups, but insertions and removals are slower due to internal hashtable operations, so as the creation of new `hash!` value from existing `block!` prototype. It should be noted that only hashable values are added to hashtable's index.

Use `hash!` for data that is updated less frequently and benefits from much faster lookups. Remember that usage of `hash!` pays off only for a large number of entries; for smaller numbers, using `block!` with key/value queries will suffice.

### `map!`

Conceptually, maps take quick lookups from `hash!` and key/value interface from `object!`. This means that `map!` values act as dictionaries with unique keys and fast queries.

Unlike objects, map's entries can be freely updated or deleted, and map itself can be populated with new key/value pairs. In newly added entries only keys are hashed, and keys are restricted to hashable values. Map follows `construct` semantics, in a sense that it does not evaluate added keys, and differs from objects in not supporting `get` and `set` (*this may change in the future*).

Unlike hashes, maps are unordered, and therefore cannot be indexed. Maps do not guarantee to preserve key order. Just like with hashes, usage of `map!` pays off only for a large number of entries.

Use `map!` if you need conventional associative array storage, and also in use-cases that involve hierarchical JSON-like data or flat-file databases.

### Algorithmic complexity

| Datatype | Search | Indexing | Insertion | Removal |
|:-|:-:|:-:|:-:|:-:|
| `series!` <br> (excluding `hash!`) | Linear | Constant | Varies <br> **(3)** | Varies  |
| `object!` | Constant <br> | Constant <br> **(2)** | _N/A_ | _N/A_ |
| `hash!` | Constant <br> **(1)** | Constant | Varies + hashing of each inserted value (if hashable) | Varies + updating hashtable |
| `map!` | Constant | _N/A_ | Constant + hashing of each key | Constant + updating hashtable |

1. For hashable values only, linear for the rest.
1. As was said earlier, this does not mean that series actions can be applied to objects. `any-word!`s are bound to `object!` values and contain internal indices, which specify the offset of `any-word!`'s value in an object to which it is bound. <br> The aforementioned binding and indexing information is used to get values referred by words in constant time.
1. Insertion or removal may result in series expansion or compaction, respectively.
    - In the **worst-case** scenario, series' buffer needs to be moved to a new memory location (proportional to new series' size);
    - In the **average case**, elements need to be shifted, either to make space for inserted value or to fill the gaps left after removal (proportional to the number of elements inserted / removed);
    - In the **best-case** scenario (appending to or taking from the tail), only adjustment of series' internal pointers is required (constant).