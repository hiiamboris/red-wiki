# How Red works: a brief explanation

_Originally Posted on [Hacker News](https://news.ycombinator.com/item?id=18843544) by 9214_

Red (and Rebol) are based on research in denotational semantics that Carl Sassenrath did. I'll try to briefly explain the main points.

Everything starts with a UTF-8 encoded string. Each valid token in this string is converted to an internal data representation - a boxed structure 4 machine pointers in size, called a _value slot_ or sometimes a _cell_.

Value slot is composed of a _header_ and a _payload_. Header contains various flags and datatype ID, payload specifies exact content of the value. If content doesn't fit in one value slot, then payload contains a pointer to an external _buffer_ (an array of value slots, bytes, or other units + offset and start/end addresses IIRC) with extra data.

So, lexer converts string representation to a tree of value slots (this phase is called _loading_), which is essentially a concrete syntax tree (CST) — this is the crux of homoiconicity.

```red
>> "6 * 7"
== "6 * 7"
>> type? "6 * 7"
== string!
>> load "6 * 7"
== [6 * 7]
>> type? load "6 * 7"
== block!
>> first load "6 * 7"
== 6
>> type? first load "6 * 7"
== integer!
```

Everything is a (first-class) value, and every value has a datatype (we have roughly 50 of them right now). And there's no code — only this data structure, which is just a block, which you can freely manipulate at will (so as any other value).

```red
>> reverse [6 * 7]
== [7 * 6]
>> append reverse [6 * 7] [+ 1]
== [7 * 6 + 1]
>> skip append reverse [6 * 7] [+ 1] 2
== [6 + 1]
```

What interpreter does is just a "walk" over this tree of values, dictated by a set of simple evaluation rules (expressions are evaluated left to right, operators take precedence over functions and have a tighter left side, literals evaluate to themselves, functions are applied to a fixed set of arguments, symbolic values of type `set-word!` [more on this later] are bound to the result of expression that follows them, etc.) but there are a couple of catches.

The first catch is that some values are symbolic — that is, they indirectly refer to some other values via a _context_ (namespace). You can modify this reference (called _binding_) freely at runtime, and thus change the meaning of symbolic values and of an entire block that contains them.

So, the "meaning" of a given block is always relative to some context(s) — this is what _relative expression_ means (RE in REBOL). And context itself is just an environment of key/value pairs (key is a "symbol", value is its "meaning") represented as an object (O in REBOL).

```red
>> block: [6 * 7]             ; BLOCK: is a value of type SET-WORD!
== [6 * 7]
>> type? second block
== word!                      ; words are one of the symbolic values I've mentioned
>> do block
== 42
>> bind block object [*: :+]  ; now "multiplication" means "addition"
== [6 * 7]
>> do block
== 13
```

The second catch is that you are not restricted by default interpreter (represented by `do` function) and can use any other one or even implement your own, thus making an embedded DSL - a _dialect_ in Red/Rebol parlance.

* Red/System takes a block of C-level code and does the prescribed job.
* View takes a block that specifies GUI layout and shows a fancy window.
* Draw takes a block of drawing commands and renders an image.
* Parse takes an input series and a block of PEG grammar and parses the input.
* Math takes a block and interprets it with common operator precedence rules.

The sky is the limit, and its dialects all the way down.

To reiterate: the basic building block (no pun intended) is a block of values, which can represent either code (relative expression which, upon evaluation, will yield a value) or data (just a bunch of values arranged in a specific format — such micro-formats are considered to be dialects too **†**). Block can also contain symbolic values (called words) which can change their binding during the evaluation, and thus alter the semantics of expression.

There's a lot hiding behind the facade, as you can see. And what is there is hardly an ad-hoc hodge-podge slapped together.

---

**†** one example of such micro-format dialect is function specification, e.g.

```red
spec: [
  "Add two numeric values together"
  x [number!]
  y [number!]
]
```

is a specification (or an "interface") for a function that performs addition. Here's a block that expresses addition of two specific numbers:

```red
[1 + 2]
```

If we wish to abstract over it, we can substitute 1 and 2 for words:

```red
expression: [x + y]
```

And then we can alter bindings of these words to actual arguments we wish to add together:

```red
bind expression object [x: 1 y: 2]
```

We then can evaluate such expression and yield a resulting value:

```red
== do expression
>> 3
```

The trick is that functions are just abstraction over evaluation of expression in some environment, that is, a syntax sugar for

```red
do bind [...] object [...]
```

with some additional optimizations and type-checking. So, addition instead can be expressed as:

```red
>> add: func spec expression
== func [
"Add two numeric values together" 
x [number!] y [number!]
][x + y]

>> type? :add ; :ADD is a value of type GET-WORD! which, on evaluation, yields function's value referred by word ADD as-is, without triggering its application.
== function!
>> add 1 2
== 3
```

---

What `load` essentially does - it takes a string and returns a [concrete syntax tree](https://en.wikipedia.org/wiki/Parse_tree) (or CST for short). `do` then takes that CST and evaluates it. You can do that in a single-step manner with `do/next` (there's also `load/next`). Moreso, you can think of blocks as phrase markers (see [wiki example](https://en.wikipedia.org/wiki/Parse_tree)):

```
[S [NP John] [VP [V hit] [NP the [N ball]]]]
```

Now compare it to Red internal data structure written down in similar fashion:

```
<block! [<integer! 1> <word! +> <integer! 2>]>
```

Here I loosely follow phrase marker notation - nonterminals correspond to datatypes, terminals correspond to literal values. But datatype is implied by literal form, and implicitly contained in each value slot (its a datatype ID tag in the header), so I can tidy this up to:

```red
[1 + 2]
```

This is what lexer returns — just a block. You can then manipulate it whichever way you want and evaluate.