Red is unique in a subtle way that is invisible in normal use, but very powerful when you know it's there, and makes you think differently about how to view data. Here's a question from a community member:

> How does block keep its structure (newlines and spaces) here?
```red
red>> f: func [a b] [
[    ; newline
[    a + b
[    print "test"
[    0 ]
== func [a b][a + b 
    print "test" 
    0
]
red>> body-of :f
== [a + b 
    print "test" 
    0
]
 red>> b: [ 1 2
[    3 4
[    ]
== [1 2 
    3 4
]
red>> b
== [1 2 
    3 4
]
```

The answer is that blocks maintain line markers, which you can detect and control with the `new-line` and `new-line?` functions. Given the above, you can find the line markers this way:

```red
red>> forall b [ probe new-line? b]
false
false
true
false
== false
```

And because they are maintained in loaded Red data, you can use it to find out how many lines of code any function has:
```red
red>> length? split mold body-of :math "^/"
== 14
```

That is `mold` is aware of the line markers, and converts them to `newline` chars.

Line markers at the tail are a special case because of how the feature is implemented. The block does not contain the markers itself, each `cell`, or value slot, does. So `new-line?`can ask, in the context of a block, if the next cell has its line marker bit turned on. There is no value at the tail of the block, which is why the `forall` example above doesn't return true for the final result.

It's a nice feature, and new-line is very handy when analyzing data sometimes, and when generating code or data. Let's say you want to break up a block of data into 3 columns. You could build up a string yourself, inserting line breaks, or you can just insert line markers and `mold` the result. The big difference is that your block is now "formatted" this way in Red. Without newline markers, a block would always display as single line. You can also use this to help visualize data and debug. It lets you generate formatted data to exchange.

```red`
>> new-line/skip [1 2 3 4 5 6 7 8 9 10 11 12] on 3
== [
    1 2 3 
    4 5 6 
    7 8 9 
    10 11 12
]
````

In the context of a data exchange language, formatting has value. That is, when you exchange information, formatting  matters. Sometimes very much. Remember, we're not just exchanging data with machines, but with other people. And while tools could address this to some extent, that means every recipient of the data then has to have those tools.

If you really want to twist your mind a bit, think about this. Can those markers be used by analysis tools, or even in dialects themselves, so an evaluator can "see" them? You can see that the values themselves carry their line marker with them:
```red
>> a: [ 1 2 3 4]
== [1 2 3 4]
>> b: [
[    a
[    b
[    c
[    d
[    ]
== [
    a 
    b 
    c 
    d
]
>> swap a b
== [
    a 2 3 4
]
>> b
== [1 
    b 
    c 
    d
]
>> swap at a 3 at b 3
== [
    c 4
]
>> a
== [
    a 2 
    c 4
]
>> b
== [1 
    b 3 
    d
]
```