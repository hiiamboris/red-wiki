### Introduction

Red provides a flexible set of aggregate structures, each coming with its own trade-offs. Said trade-offs are not entirely obvious to beginners and newcomers, who frequently ask about the difference between `block!` and `hash!`, `hash!` and `map!`, `map!` and `object!`.

This page addresses respective pros, cons and common use-cases of these datatypes.

### Table of contents

1. [Phrasebook](#phrasebook)
1. [`block!`](#block)
1. [`object!`](#object)
1. [`hash!`](#hash)
1. [`map!`](#map)
1. [Algorithmic complexity](#algorithmic-complexity)

### Phrasebook

- *Indexing operations* — `at`, `pick`, `poke` and the like, path accessors with `integer!` keys;
- *Key/value queries* — `select`, `put`, and path accessors with non-`integer!` keys;
- *Hashable* — `scalar! all-word! any-string!`.

### `block!`

Blocks are a bread and butter of Red programming used in all facets of coding: from daily scripting, records and data structures to metaprogramming and dialects.

Blocks are completely free-form and can hold values of any type, providing a convenient `series!` interface for their access.

Series are sequential by nature, and search in them takes linear time, so as key/value queries; however, indexing operations take constant time, due to `series!`' internal optimizations.

Use `block!` whenever you need a general-purpose container that you frequently update, shrink, expand and pass around between functions.

### `object!`

Just like blocks, objects form backbone of the language. Red's object model is influenced by prototype-based and reactive programming, without strictly adhering to an Object-Oriented paradigm. In this way, objects cannot form inheritance chains and act only as namespaces, grouping related values together and following information hiding principle.

Objects provide a key/value interface and cannot be indexed with `series!` actions; its keys are strictly limited to `set-word!` datatype, and accessing its values via bounded words takes constant time.

Searching for `any-word!` value in object takes linear time. Once created, objects cannot be extended with new entries (*this may change in later versions*), nor can existing entries be deleted; however, object can be used as a prototype for another object.

Use `object!` to group functionally-related data together and encapsulate your code's logic. Don't follow classic OOP principles too strongly; instead, seek to leverage `object!`s in idiomatic ways: by using reactors, bindings and reflection.

### `hash!`

Hashes are ordered series, and can be queried and indexed just like blocks, without imposing any limitations on types of stored values.

Unlike blocks, hashes have much faster lookups, but insertions and removals are slower due to internal hashtable operations, so as creation of new `hash!` value from existing `block!` prototype. It should be noted that only hashable values are added to hashtable's index.

Use `hash!` for data that is updated less frequently and benefits from much faster lookups.

### `map!`

Conceptually, maps take quick lookups from `hash!` and key/value interface from `object!`. This means that `map!` values act as dictionaries with unique keys and fast queries.

Unlike objects, map's entries can be freely updated or deleted, and map itself can be populated with new key/value pairs. In newly added entries only keys are hashed, and key's are restricted to hashable values.

Unlike hashes, maps are unordered, and therefore cannot be indexed. Maps do not guarantee to preserve key order.

Use `map!` if you need a conventional associative array storage, and also in use-cases that involve hierarchical JSON-like data or flat-file databases.

### Algorithmic complexity

| Datatype | Search | Indexing | Insertion | Removal |
|:-|:-:|:-:|:-:|:-:|
| `series!` (excluding `hash!`) | Linear | Constant | Varies **(3)** | Varies  |
| `object!` | Linear | Constant **(2)** | _N/A_ | _N/A_ |
| `hash!` | Constant **(1)** | Constant | Varies + hashing of each inserted value (if hashable) | Varies + updating hashtable |
| `map!` | Constant | _N/A_ | Constant + hashing of each key | Constant + updating hashtable |

1. For hashable values only, linear for the rest.
1. As was said earlier, this does not mean that series actions can be applied to objects. `any-word!`s are bound to `object!` values and contain internal indices, which specify the offset of `any-word!`'s value in an object to which it is bound. <br> Aforementioned binding and indexing information is used to get values referred by words in constant time.
1. Insertion or removal may result in series expansion or compaction, respectively.
    - In the **worst-case** scenario, series' buffer needs to be moved to a new memory location (proportional to new series' size);
    - In the **average case**, elements need to be shifted, either to make space for inserted value or to fill the gaps left after removal (proportional to number of elements inserted / removed);
    - In the **best-case** scenario (appending to or taking from the tail), only adjustment of series' internal pointers is required (constant).