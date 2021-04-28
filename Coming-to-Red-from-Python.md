## Why Red?

Python is an amazing tool capable at almost every programming task imaginable, unless you have to deploy it. Oh, it can be done, no arguments there. But as soon as things get beyond "my linux box", the headaches grow exponentially. Eventually, we all just want to burn down the house. Perhaps Red can be your lighter.

You might be wondering “Why I need to learn Red, when I have Python at hand?” Well, you probably don’t need to learn yet another programming language, but before you decide for yourself, consider the following:
-	Red is interpreted, but can be compiled. The resulting executable files are around 1MB, with no dependencies.
-	Red can be cross-compiled to different platforms (Windows, WindowsXP, Linux, Linux-ARM, RPi, Darwin,  macOS, Syllable, FreeBSD, Android, Android-x86).
-	Red has a fully reactive cross-platform native GUI system, with a UI Domain-specific language (DSL) - VID and drawing DSL – Draw.
-	Red has a powerful [PEG]( https://en.wikipedia.org/wiki/Parsing_expression_grammar) parser DSL – Parse, that is highly readable and maintainable, unlike regex.
-	[Red/system](https://static.red-lang.org/red-system-specs.html) is a low level (think C) DSL, aimed at low-level system programming and higher performance. 
-	Red has an elegant and consistent syntax, allowing for easy learning. 
-	Red comes with a rich set of built-in [datatypes](https://github.com/red/docs/blob/master/en/datatypes.adoc) - more than 50.
-	All these make Red a full-stack language ("from metal to meta") that can be used for writing everything from drivers and operating systems, through applications, scripts, to Domain-specific languages and metaprogramming.

[More about Red features](https://www.red-lang.org/p/about.html)

However, Red still doesn't have Python's rich ecosystem - for example we don't have any analogues of numpy/scipi. The module system is yet to be completed, as well as a set of higher order functions. At the moment of writing Red is 32 bit. But nothing in the python ecosystem is as powerful as parse, nor is python nearly as flexible. There is nothing like trying something for yourself though so lets dig in!

## [A short introduction to Red for Python programmers](https://github.com/red/red/wiki/A-short-introduction-to-Red-for-Python-programmers)

## Common tasks and how to do them in Red

## Red functions for the Python programmers

## Draft outline
   ### A short introduction to Red for Python programmers
   #### Words ✓
   #### Evaluation order ✓
   #### Datatypes ✓
   #### Blocks and series ✓
   #### Conditionals ✓
   #### Looping ✓
   #### Functions =
   #### Misc (date, vectors, files...)
   #### View
   #### Draw
   #### Parse

## Notes

### Some examples to consider

## Potential Gotchas
### Namespaces
Red uses namespaces in their pure sense: just a way to allow the same symbol to mean different things in different contexts. There is a lot of additional baggage with the concept of a namespace though so generally Red programmers like to refer to namespaces as contexts. Contexts are straightforward but their simplicity can have consequences that seem odd. Particularly if you think of them as python-like namespaces.

#### What are Contexts?
Contexts are just lookup tables for symbols. The symbol `cookie` in context `A` might mean `"chocolate"`. Whereas in context `B`, the same symbol might mean `"vanilla"`. So far, very much like pythonic namespaces. Under the hood though, each instance of a symbol has a reference to its context. So the lookup for a symbol doesn't assume a context (namespace). In Red, the lookup for a symbol instance first reads its context, then looks up what the symbol means in that context.

(The cookie example is a good one to show that words have meaning in a context, not just different values they refer to. For example, a cookie recipe lets you make cookies, but in an HTTP context, cookie means something completely different. So if you have a web service that serves up cookies, you need to talk about both. And if you cook edible cookies, but decide to call your HTTP cookie preprocessor's action `cook` (because that's fun) then `cook cookie` might look exactly the same in code or logs, but mean very different things. --Gregg)

This means you can have two lexically identical symbols `[cookie cookie]` which will be resolved as two independent values. The first cookie's context having been set to `A` while the second set to `B`. It's pretty neat. Especially because you can do this context swapping at runtime, whereas pythonic namespaces are effectively "hard coded" at runtime.

There are some fun brain teasing examples of this, often having to do with spoons, that the community has made over the years that might help show both the power, and potential for insanity, contexts bring. Before getting to them, just know that certain keywords <insert list here> affect context for the symbols defined with them. But, most importantly: nothing lexical affects them in any way. Blocks do not create contexts. When you read this next time, trying to figure out a variable clobbering, just know that we've all been here and are happy to help you sort things out.
