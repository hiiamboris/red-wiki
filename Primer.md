When starting with a new language, there will be new things to learn, and things you expect to work the way you're used to, but don't. 

# Words evaluate by default

```Red
>> 'a
== a
>> a
*** Script Error: a has no value
*** Where: catch
>> :a
>> a:
*** Script Error: a: needs a value
*** Where: a
>> type? :a
== unset!
```

# Only `false` and `none` are falsey

Everything else, 0, empty strings, etc. is truthy. In addition, `to` and `make` don't have the same semantics when creating logic values. e.g.:

```Red
>> to logic! 0
== true
>> make logic! 0
== false
```

# `Return:` typeset is `any-type!`, not `default!`

`Default!` doesn't include `unset!`.

# Word: assigns a word to a value (not initialize to a constant)

The `set-word!` behaves a bit differently than an assignment operator in most languages. It actually links the word to the data that follows it. So when you modify a series or other modifiable value inside a function, the function itself is changed.

```Red
>> f: function [][
[    test: ""
[    append test length? test
[    ]
== func [/local test][
    test: "" 
    append test length? test
]
>> f
== "0"
>> f
== "01"
>> f
== "012"
>> body-of :f
== [
    test: "012" 
    append test length? test
]
```

If you want to initialize a new series each time, you would typically do `test: copy ""`.