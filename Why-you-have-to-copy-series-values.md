Red's designer (Nenad Rakocevic) here. Let me try to shed some light on this fundamental topic.

Red & Rebol are data formats first, before being programming languages. This has many implications and is overlooked by most newcomers. Rebol documentation did not stress this fact enough. Red will try to do a better job at that.

What does that mean in practice?

In almost all non-Lisp languages, you go from a textual representation of your source code to an AST which is compiled/interpreted. That is not the case in Red/Rebol, the text representation is LOADed as Red data and put in a block, illustrated by this example:

```
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

```
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

```
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

```
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

