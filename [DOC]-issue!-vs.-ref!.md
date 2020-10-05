### Introduction

This article briefly discusses the key differences between `issue!` and `ref!` datatypes, pointing out their respective features, drawbacks, and intended use-cases.

### Table of Contents

1. [History](#History)
1. [Rationale](#Rationale)
1. [Comparison](#Comparison)
    1. [String](#String)
    1. [Symbol](#Symbol)
1. [Summary](#Summary)

### History

Historically, `issue!` in Rebol2 was a string-like datatype, originally devised as a denotation for serial numbers and identifiers of any kind. In Rebol3 the implementation was changed to symbol-like one, which was dictated by a primary use case for `issue!` values at the time — directives in various preprocessors.

### Rationale

Red followed Rebol3 and kept `issue!` as symbolic value. Like any design choice, this one came with a set of trade-offs, encouraging certain use-cases while inhibiting the others. In that sense, `ref!` complements and balances this choice by acting like a Rebol2 string-like `issue!`, albeit with a slightly different lexical form.

Ergo, to understand the pros and cons of each datatype, the difference between a symbol and a string must be understood.

### Comparison

Values of both datatypes can be used to represent generic alphanumeric identifiers, such as name aliases, phone numbers, UUIDs &c. The key difference between them lies in the efficiency of handling large/small number of values, and in the nature of their usage.

#### String

`ref!` belongs to `any-string!` typeset, which is an umbrella term for string-like series, other examples being `tag!` or `file!`. Internally, such strings are mutable binary buffers containing UTF-8 data; they are allocated on the heap and reclaimed by the garbage collector when no longer needed.

`any-string!` values are extremely flexible and versatile but can become a performance bottleneck in cases where intensive lookup operations are needed. Note also that two strings, while being identical in form and content, still require separate buffers to hold their data.

#### Symbol

`issue!` is a part of `all-word!` typeset, which encompasses symbolic values such as `word!` or `refinement!`. Implementation-wise, symbols are [interned strings](https://en.wikipedia.org/wiki/String_interning); they are immutable objects that are represented as indices (IDs, identifiers) into a symbol table (aka string pool) in which they are stored. 

The symbol table does not live under the purview of the garbage collector, which means that memory allocated to the symbol is reclaimed only after program termination. Since symbols are represented as integer IDs, comparison between them is instantaneous — **O(1)** in contrast to **O(n)** of `any-string!`; this also means that two identical symbols resolve to the same ID and do not waste an extra space.

In addition to that, Red supports symbol aliasing, thereby case-varying symbols are linked (aliased) together in a chain that terminates with the first instance of a said symbol.

### Summary

The summary of key differences between `issue!` and `ref!` can be found in the table below.

| Datatype | `issue!` | `ref!` |
|:--|:-:|:-:|
| Typeset | `all-word!` | `any-string!` |
| Implementation | Symbol | String |
| Mutability | Immutable | Mutable |
| Comparison | **O(1)** | **O(n)** |
| Storage | Same values reuse the same symbol identifier | Same values hold their own data buffers |
| Reclamation | Permanently stored in the symbol table | Garbage collected |

An outline of practical use-cases for these datatypes is as follows:

* `issue!` — a relatively small, fixed set of symbols that are frequently searched and tested for identity (e.g. keywords, keys in associative arrays);
* `ref!` — a large number of diverse identifiers that are created and operated upon at runtime (e.g. social media handles, generic identifiers).