# Creation

(@PeterWAWood @rebolek @greggirwin)

Is there a reason you can't initialize a word in a context with the same name?

```
>> a: 5
== 5
>> c: context [a: a]
*** Script Error: a has no value
*** Where: a
*** Stack: context
```

Here is why you get the error:

```
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

```
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

```
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


