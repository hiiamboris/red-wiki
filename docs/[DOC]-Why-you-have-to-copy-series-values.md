# A real (Hello) world example.

I write a niiiice function:
```
>> f: func [] [print append "hello " "world"]
```
It works!:
```
>> f
hello world
```
Or does it?
```
>> f
hello worldworld
>> f
hello worldworldworld
>> f
hello worldworldworldworld
```
What the hell???!!?!
```
>> source f
f: func [][print append "hello worldworldworldworld" "world"]
```
That's not the function I wrote!! I'm being hacked!! :(

### So what's the problem?

Now watch the process as it happens. The very fundamentals explained step by step.

```
>> f: func [] [print append "hello " "world"]
```
That above is **not a function** yet. Until I press `<Enter>` in console, it's just a *string!* of 42 chars (hmm what a number).

When I press `<Enter>`, console internally `load`s the string: this is when chars become Red data. Let's imitate that:
```
>> load {f: func [] [print append "hello " "world"]}
== [f: func [] [print append "hello " "world"]]
```
So from `load` I got a *block!* `[f: func [] [print append "hello " "world"]]`.

But hold your horses! In this block there is no function yet:
```
>> foreach value load {f: func [] [print append "hello " "world"]} [?? value]
value: f:
value: func
value: []
value: [print append "hello " "world"]
```
Pfft. This is **just data**. 4 values. Nothing interesting, except that some of these values are *series!*. Series include blocks and strings, so there are 4 series in the loaded block `[f: func [] [print append "hello " "world"]]`:
- `[]`
- `[print append "hello " "world"]`
- `"hello "`
- `"world"`

**4 series** loaded! Remember that!

Now, the magical moment when we start thinking of our data as code happens when we pass our data to the `do` function. **All code is data before it's evaluated!** And `do` does the evaluation.

Next step console peforms on loaded string is evaluates it with `do`. Let's try that manually:
```
>> data: [f: func [] [print append "hello " "world"]]
>> do data
== func [][print append "hello " "world"]
```
Note: what `==` returns is a single value. Which is now assigned to `f`:
```
>> :f
== func [][print append "hello " "world"]
>> type? :f
== function!
```
Now I have a single *function!* instead of the 4 values. How so?

`func` is a function constructor that took 2 blocks (`[]` and `[print append "hello " "world"]`) as arguments and created a *function!*. But as you can see from `:f` output, `f` still has all those 4 loaded series!

And the key is... **`func` did not copy any of that!** OK, almost. It did actually copy the body, but not deeply, so strings remained the same:
```
>> last data
== [print append "hello " "world"]
>> last last data
== "world"
>> body-of :f
== [print append "hello " "world"]
>> last body-of :f
== "world"
>> same? (last last data) (last body-of :f)
== true
```
So `append "hello "` appends directly to the `"hello "` string which happened to slip into the function's body block. Modifies it in place **every time function is called**.

### How to remedy?

**Use a copy!** 

Each series you see is actually a pointer to some memory location where it's data is held. When I write:
```
>> other: data
```
I don't copy the data, I just create another word (`other`) pointing to the same data:
```
>> same? other data
== true
```
So everywhere in my code where I have a link to that `data`, I will be modifying the same thing in memory. 
`copy` can help by copying the data and returning a link to it's copy:
```
>> other: copy data
>> same? other data
== false
```
Now that I know how `copy` works, and I know I'm modifying the `"hello "` string above, I can make a string's copy so I would not modify the original:
```
>> f: func [][print append copy "hello " "world"]
== func [][print append copy "hello " "world"]
>> f
hello world
>> f
hello world
>> f
hello world
```
It works! Woohoo!! Now I'm a Red ace, envy you greenies ;)

For a deeper explanation go ahead and read below. A lot of super useful info there if you're ready to grok it!

# A designer's view

Red's designer (Nenad Rakocevic) here. Let me try to shed some light on this fundamental topic.

Red & Rebol are data formats first, before being programming languages. This has many implications and is overlooked by most newcomers. Rebol documentation did not stress this fact enough. Red will try to do a better job at that.

What does that mean in practice?

In almost all non-Lisp languages, you go from a textual representation of your source code to an AST (abstract syntax tree) which is compiled/interpreted. That is not the case in Red/Rebol, the text representation is LOADed as Red data and put in a block, illustrated by this example:

```red
red>> src: {test: func [input /local s][s: "hello" append s input]}
== {test: func [input /local s][s: "hello" append s input]}
red>> code: load src
== [test: func [input /local s] [s: "hello" append s input]]
red>> code/4/2
== "hello"
red>> append code/4/2 " world"
== "hello world"
red>> code
== [test: func [input /local s] [s: "hello world" append s input]]
red>> length? code
== 4
```

"The truth is that...there is no spoon!" or rather "there is no code", as everything is data. This has deep implications. For example, every function body is just a block of data, that you can manipulate as any other data, and you can do so *at any time*. There is no point in time where your "source code" becomes "code". It is, and remains, data.

To get the correct mental picture of what happens when you evaluate such a test function, do not try to apply your knowledge of execution models from other languages to Red/Rebol. That will just block you from learning and understanding Red.

Always keep in mind that the lines of "code" you are reading are just "data structures", nothing else.

Strictly speaking there are no "variables" in Red. Words are first-class values, with their own datatype, and can refer to another value in a given context (we call that *binding*). You can think of it as a simple link between the word and a given value. Therefore `a: "string"` just makes the word `a` refer to the literal string which follows it, nothing else.

Let's run the test function and see what happens.

```red
red>> do code
== func [input /local s][s: "hello world" append s input]
red>> test "x"
== "hello worldx"
red>> body-of :test
== [s: "hello worldx" append s input]
```

First, the code block needs to be evaluated in order for the `function!` value to be constructed in memory. This is achieved by `do`. Inside the `function!` value, the spec and body blocks are still there, and they are still data. So, when `s` is evaluated, it evaluates to the `string!` value which is the 2nd element of the function body block. Hence, every modification applied to the value referred by `s`, changes that same `string!` value.

It should be clear, now, that each time you evaluate the function, you are modifying a literal series (string, block, ...) in its body block. You need to `copy` it if you make such a modification more than once and want to keep the original series untouched, or otherwise construct a series dynamically (`make string! 10` for example) instead of using a literal one.

# Deeper into the rabbit hole

```red
red>> :test
== func [input /local s][s: "hello worldx" append s input]
red>> clear second body-of :test
== ""
red>> :test
== func [input /local s][s: "" append s input]
red>> code
== [test: func [input /local s] [s: "" append s input]]
red>> test "x"
== "x"
red>> :test
== func [input /local s][s: "x" append s input]
red>> code
== [test: func [input /local s] [s: "x" append s input]]
```

As you can see, you can easily modify the content of the function (it's just data), and you can inspect the change even in the unevaluated code block, because the function body block was not copied with `copy` (for performance reasons, though you can manually force the copy when required).

This property can be leveraged in functions to implement local caches, for free, though you can use it for many other applications (code hot-swapping, live debugging a running program, etc...).

In general, this code-as-data approach makes serialization trivial, so saving snapshots of "code" from a running program to disk or sending code remotely, are one-liners in Red/Rebol.

```red
red>> code
== [test: func [input /local s] [s: "x" append s input]]
red>> save %my-code.red code
red>> blk: load %my-code.red
== [test: func [input /local s] [s: "x" append s input]]
red>> do blk
== func [input /local s][s: "x" append s input]
red>> test "o"
== "xo"
red>> :test
== func [input /local s][s: "xo" append s input]
```

One more thing. The colon (`:`) suffix in a word is not an assignment operator (as in other languages), it's part of the `set-word!` datatype literal syntax. When evaluated, it binds the word to the result of next expression. It doesn't do anything more than that. So `a: ""` does not "reset" or "reinitialize" the "variable" `a`. That is an incorrect interpretation, it just makes the word `a` refer to the literal string `""` which follows it.

