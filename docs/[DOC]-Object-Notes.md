# Creation

(@PeterWAWood @rebolek @greggirwin)

Is there a reason you can't initialize a word in a context with the same name?

```red
>> a: 5
== 5
>> c: context [a: a]
*** Script Error: a has no value
*** Where: a
*** Stack: context
```

Here is why you get the error:

```red
>> a: 5
== 5
>> o: make object! [
[    probe context? 'a
[    a: 1
[    probe context? 'a
[    ]
make object! [
    a: unset
]
make object! [
    a: 1
]
== make object! [
    a: 1
]
```

The word `a` is first added to the context to which `o` is bound. By the time `a: a` is evaluated `a` exists in the context but is unset.

You need to supply the context of the `a` to which you wish to bind the `a` in the object context. For example, you can reference the global context or use the `in` function:

```red
>> a: 5
== 5
>> f: func[a][print a]
== func [a][print a]
>> o: make object! [a: system/words/a   f: get in system/words 'f   f a]
5
== make object! [
    a: 5
    f: func [a][print a]
]
```

Here is a way to achieve the same result with compose:

```red
>> a: 5
== 5
>> f: func[a][print a]
== func [a][print a]
>> c: make object! compose/deep [a: (a)  f: quote (:f)  f a]
5
== make object! [
    a: 5
    f: func [a][print a]
]
```

# `construct` vs `context`/`object`

How is `context []` and `make a []` different from `construct []` and `construct/with [] a` ?

Construct is a low-level constructor that *does NOT*:
- evaluate inner expressions
- bind the spec block to the context (not always true, see the note below)


```red
USAGE:
     CONSTRUCT block

DESCRIPTION: 
     Makes a new object from an unevaluated spec; standard logic words are evaluated. 
     CONSTRUCT is a native! value.

ARGUMENTS:
     block        [block!] 

REFINEMENTS:
     /with        => Use a prototype object.
        object       [object!] "Prototype object."
     /only        => Don't evaluate standard logic words.
```

It allows one to achieve fine-tuned context composition, otherwise broken by the implicit `bind`:
```red
>> x: 'x-global  a: construct compose [x: x-of-a f: (does [x])]  a/f
== x-global
```
Above, the `f` function body retains it's binding to the global (system/words) context. Compare that to `context`:
```red
>> x: 'x-global  b: context [x: 'x-of-b  f: does [x]]  b/f
== x-of-b
```

As of Red v0.6.3 however composition is unreliable yet when it comes to bindings. More info for documenting it here later once it's resolved can be found at: https://redd.it/8g7oce . See related github issues for update on the progress: https://github.com/red/red/issues/3406 https://github.com/red/red/issues/3365

More valuable observations in Gitter: [March 31, 2019 11:25 PM](https://gitter.im/red/red?at=5ca12229f851ee043d3cc415)


# `Return` in spec blocks

`Context`, by design, processes a call to `return` in the spec block. It returns the argument value of the called return instead of the object.

```red
>> o: context [x: 5 return 2 + x]
== 7
>> probe o
7
== 7
```

The rest of the block (after `return`) is not evaluated:
```red
>> context [return 1 probe 2]
== 1
```

But the set-words are still collected:
```red
>> context [return self x: 1]
== make object! [
    x: unset
]
```

# Conversions with `map!`

Should we be able to convert objects and maps into each other using to or make (or at least object into map, as ordered into orderless collection)?
```
>> make map! object [a: 1]
*** Script Error: cannot MAKE/TO map! from: make object! [a: 1]
*** Where: make
*** Stack:  

>> object #(a: 1)
*** Script Error: object does not allow map! for its spec argument
*** Where: object
*** Stack: object
```

From the designer:

Map->object: would only work on a subset of possible maps.

Object->map: binding info would be lost, making evaluation of code held inside the map error out.

You can still use an intermediate block to force conversions in such cases, if really needed. You can also write a routine if memory usage or performance is a show-stopper.
