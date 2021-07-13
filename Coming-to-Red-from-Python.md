## Why Red?

Python is an amazing tool capable at almost every programming task imaginable. So it's logical to ask “Why should I learn Red, when I have Python at hand?” One reason is that it's good for your brain. Stretch yourself and learn something, anything new. Red and Python are _very_ different languages and you may find ideas or techniques that you can put in your Python toolbox. We also point out that Red is fun to play with. Like Python, you can tinker easily. Sometimes it helps to take a break from real work in your day-job language, but keep your brain in the _realm_ of programming. Play with ideas out of context and you may have sudden insights; the proverbial a-ha moment. Finally, there are things Red is really good at. You may find you like doing things in Red that Python doesn't focus on.

Consider the following:

-	Red is interpreted, but can be compiled. The resulting executable files are around 1MB, with no dependencies. The "no dependencies" part is important if you want to easily share them. Nothing for users to do but drop your exe in a folder.
-	Red can be cross-compiled to different platforms (Windows, WindowsXP, Linux, Linux-ARM, RPi, Darwin,  macOS, Syllable, FreeBSD, Android, Android-x86) _from_ any platform. That's built in. No external compilers, hardware, or setup necessary.
-	Red has a fully reactive cross-platform native GUI system called [View](https://github.com/red/docs/blob/master/en/view.adoc), with a UI DSL [VID](https://github.com/red/docs/blob/master/en/vid.adoc) and vector drawing DSL called [Draw](https://github.com/red/docs/blob/master/en/draw.adoc).
-	Red has a powerful [PEG](https://en.wikipedia.org/wiki/Parsing_expression_grammar) DSL – [Parse](https://github.com/red/docs/blob/master/en/parse.adoc), that is highly readable and maintainable. Via `parse`, Red is really a language construction toolkit.
-	[Red/system](https://static.red-lang.org/red-system-specs.html) is a low level (think C) DSL, aimed at low-level system programming and higher performance. Where other scripting languages require you to write extensions in C (for which you need a compiler and a different syntax in your head), in Red the only thing that changes is the semantics. Basically, you give up high level language features at the system level, but have total control.
-	Red has an elegant and consistent syntax, and both GUI and console mode REPLs, allowing for easy learning. 
-	Red comes with a rich set of built-in [datatypes](https://github.com/red/docs/blob/master/en/datatypes.adoc) - more than 50. Red is really a "data first" language, and is fully homoiconic. That means it is its own data format. So while you can use JSON and CSV (their codecs are built in), you'll only ever need to (or want to) for interoperability.
-	Red is a full-stack language ("from metal to meta") that can be used for writing everything from drivers and operating systems, through applications, scripts, to domain-specific languages and metaprogramming.

[More about Red features](https://www.red-lang.org/p/about.html)

However, and importantly, Red doesn't yet have Python's rich ecosystem - for example there are no analogues of numpy/scipi. The module system is yet to be completed, as well as a set of higher order functions. At the moment of writing Red is 32-bit. But nothing in the Python ecosystem is as powerful as `parse` for building DSLs, nor is python nearly as flexible. There is nothing like trying something for yourself though so lets dig in!

## [A short introduction to Red for Python programmers](https://github.com/red/red/wiki/A-short-introduction-to-Red-for-Python-programmers)

## [Common tasks and how to do them in Red](https://github.com/red/red/wiki/Common-tasks-and-how-to-do-them-in-Red-%28for-Python-programmers%29) 

## [Red functions for Python programmers](https://github.com/red/red/wiki/Red-functions-for-Python-programmers)

## Notes

## Potential Gotchas
### Namespaces
Red uses namespaces in their pure sense: just a way to allow the same symbol to mean different things in different contexts. There is a lot of additional baggage with the concept of a namespace though so generally Red programmers like to refer to namespaces as contexts. Contexts are straightforward but their simplicity can have consequences that seem odd. Particularly if you think of them as python-like namespaces.

#### What are Contexts?
Contexts are just lookup tables for symbols. The symbol `cookie` in context `A` might mean `"chocolate"`. Whereas in context `B`, the same symbol might mean `"vanilla"`. So far, very much like pythonic namespaces. Under the hood though, each instance of a symbol has a reference to its context. So the lookup for a symbol doesn't assume a context (namespace). In Red, the lookup for a symbol instance first reads its context, then looks up what the symbol means in that context.

(The cookie example is a good one to show that words have meaning in a context, not just different values they refer to. For example, a cookie recipe lets you make cookies, but in an HTTP context, cookie means something completely different. So if you have a web service that serves up cookies, you need to talk about both. And if you cook edible cookies, but decide to call your HTTP cookie preprocessor's action `cook` (because that's fun) then `cook cookie` might look exactly the same in code or logs, but mean very different things. --Gregg)

This means you can have two lexically identical symbols `[cookie cookie]` which will be resolved as two independent values. The first cookie's context having been set to `A` while the second set to `B`. It's pretty neat. Especially because you can do this context swapping at runtime, whereas pythonic namespaces are effectively "hard coded" at runtime.

There are some fun brain teasing examples of this, often having to do with spoons, that the community has made over the years that might help show both the power, and potential for insanity, contexts bring. Before getting to them, just know that certain keywords <insert list here> affect context for the symbols defined with them. But, most importantly: nothing lexical affects them in any way. Blocks do not create contexts. When you read this next time, trying to figure out a variable clobbering, just know that we've all been here and are happy to help you sort things out.

Pragmatically, this design notion comes from Red being a *data format* first ([code is data and data can be used as code](https://en.wikipedia.org/wiki/Homoiconicity)). To mimick scoping at data level, context has to be implicit. As a consequence, any block of code e.g. `[take the spoon]` can be passed around, evaluated on demand in any place, used in user-defined custom loops, without losing the original meaning even if all it's words `take`, `the` and `spoon` were defined differently in those loops or other functions.