### Introduction

Key points that need to be understood before evaluating Red on the merits of its type system:
1. Red is both a data format **and** a programming language, i.e. it is [homoiconic](https://en.wikipedia.org/wiki/Homoiconicity);
1. Red is both interpreted **and** compiled;
1. Red has a rich type system (**+50** datatypes) and a **high** degree of polymorphism;
1. **Everything** in Red is a value; **each** value is a [first-class citizen](https://en.wikipedia.org/wiki/First-class_citizen) with a datatype and zero **`*`** or more **unique** literal forms.
1. Red does **not** have "scopes" or "variables", in the sense in which these terms are used in the mainstream languages.

### Classification

With that being said, Red's type system loosely falls in the following categories, from general to more specific:

| Category | Description |
|:-|:-|
| Dynamic | Each value has an associated type tag; type verification is carried at run-time by the interpreter. |
| Strong | Type declarations affect only parameters of functions and are used to establish the invariants; there are no implicit type conversions. |
| Inferred | Type of value is "inferred" from its literal form at load-time (i.e. during conversion from textual source to in-memory data format); syntax determines semantics. Also, it is possible to construct/convert a value from/to a given type at run-time. |
| Structural | Comparable types share the same structural elements (at the level of in-memory data format). Types that share a subset of features are grouped together into [type hierarchy](https://github.com/toomasv/red-type-hierarchy). |
| Gradual | Compiler performs a simple form of static checking **`†`**, the rest is delegated to run-time. |
| Latent | Types are associated with values, not "variables". What tends to be misrepresented as "variable" is itself a value with a datatype (see [key point **#4**](#Introduction)). |

### Points of leverage

The major source of leverage in Red comes from a combination of one or many [key points](#Introduction) with:

1. Availability of powerful PEG-like [parser](https://github.com/9214/docs/blob/parse/en/parse.adoc) embedded in the language, which allows verification/validation of composite values in accordance with a specific format;
1. Support for the object-oriented reactive [model](https://doc.red-lang.org/en/reactivity.html) and ownership [system](https://www.red-lang.org/2016/03/060-red-gui-system.html) — this permits tracking, discarding, undoing and redoing changes applied to data structures, making possible implementations of constraint-based testing frameworks, design-by-contract and various forms of self-protecting and self-healing code;
1. Emphasis on linguistic abstraction, which encourages the creation of embedded domain-specific languages (dialects) with their own type discipline (e.g. [Red/System](https://static.red-lang.org/red-system-specs.html)) or extension of the base language (e.g. [dependent types](http://red.qyz.cz/dependent-types.html));
1. Embeddability, whereby Red can be used from the confines of a preferred type system via [libRed API](https://doc.red-lang.org/en/libred.html).**`‡`**

It is worth remembering that type system is a formalized set of self-imposed constraints, which aids programmers in dealing with the essential complexity of a given task. Knowing that and having points of leverage at their disposal, idiomatic Red programmers design their _own_ culture and discipline of programming, based on the first-hand experience with the problem domain.**`§`**

---

**`*`** Because of the tight lexical space, some values don't have literal forms. This is one of the main challenges that tends to be overlooked in discussions of user-defined datatypes.

**`†`** To the extent possible in the light of Red's highly dynamic nature.

**`‡`** Due to a wide gap between type systems of Red and other languages, this typically narrows the range of usable datatypes to integers, strings, floats, and boolean values.

**`§`** Cf. Seymour Papert's concept of microworlds; Rebol (and therefore Red) are influenced by Logo in this regard.