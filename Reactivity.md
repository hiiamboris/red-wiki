Primary reactivity docs are at https://doc.red-lang.org/en/reactivity.html

Notes on single triggering: https://github.com/red/red/issues/3096

# Making reactors

When making reactors, you have control over how reactors made from the same spec behave. That depends on whether you `copy/deep` the spec, or not. If you have read https://github.com/red/red/wiki/Why-do-I-have-to-copy-series-values%3F, you know that Red is a data format first. If you have the same spec, but it appears multiple times as literal data, each is unique. If you use a word to refer to the *same* spec block, that same data is used. This is important in the reactivity subsystem. 

In this first example, we make 3 reactors from 3 different spec blocks. The blocks look the same, but each is a separate value. As you can see, each reactor is independent of the others, and likely the behavior you expect.

```
>> a: make reactor! [x: 1 y: 2 total: is [x + y]]
== make object! [
    x: 1
    y: 2
    total: 3
]
>> a/x: 100
== 100
>> a/total
== 102
>> 
>> b: make reactor! [x: 1 y: 2 total: is [x + y]]
== make object! [
    x: 1
    y: 2
    total: 3
]
>> b/x: 200
== 200
>> b/total
== 202
>> a/x: 111
== 111
>> a/total
== 113
>> 
>> c: make reactor! [x: 1 y: 2 total: is [x + y]]
== make object! [
    x: 1
    y: 2
    total: 3
]
>> c/x: 300
== 300
>> c/total
== 302
>> b/x: 222
== 222
>> b/total
== 224
>> a/total
== 113
```

But what happens if the *same* spec is used to make multiple reactors. Here we set `spec` to reference the spec block, and re-use that for a second reactor (`b`). Everything looks normal, until we set `b/x:` and then examine `a/total`. Because the same spec block was used, the reactive subsystem connected it to the last reactor that was made from it. You can see that changes to `a/x` no longer trigger reactions in `a/total`, but changes to `b/x` are reflected in `a/total`. This is likely *not* the expected behavior. But Red can't read your mind. It *might* be what you want, so you can do it. And the way to produce the behavior from the first example is to use `copy/deep` on shared specs you use to make reactors. The `/deep` refinement is important here, because it's the `is` sub-block that you really need to copy. When we do this for the third reactor below (`c`), you can see that `b` does not suffer the same fate as `a` did.

```
>> spec: [x: 1 y: 2 total: is [x + y]]
== [x: 1 y: 2 total: is [x + y]]

>> a: make reactor! spec
== make object! [
    x: 1
    y: 2
    total: 3
]
>> a/x: 100
== 100
>> a/total
== 102
>> 
>> b: make reactor! spec
== make object! [
    x: 1
    y: 2
    total: 3
]
>> b/x: 200
== 200
>> b/total
== 202
>> a/x: 111
== 111
>> a/total
== 202
>> 
>> c: make reactor! copy/deep spec
== make object! [
    x: 1
    y: 2
    total: 3
]
>> c/x: 300
== 300
>> c/total
== 302
>> b/x: 222
== 222
>> b/total
== 224
```

# Future work

If you are interested in building a new, more sophisticated reactive library for Red (as suggested in red/red#3096), you are welcome. The current one serves its purpose very well, but it might not be good enough for more complex use-cases. One difficult part would be designing a new API which is as lightweight as possible; ideally as simple as current one, but that may not be possible.

