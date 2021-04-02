People coming to Red from other languages often suggest that Red should adopt features from that language. The problem is that a language is not just a collection of features. You can't just drop new ones in and make the language better. You have to consider the language as a whole, its goals and aesthetics; its foundational principles and the conceptual integrity as viewed through different lenses. That doesn't mean Red can't adopt new features, just that it generally isn't quick and easy. Features make languages different, but that's not the same thing as making them better.

As language trends change, certain feature requests will come up again and again, repeatedly, over and over. To avoid endless chat that changes only slightly on each iteration, and to collect thoughts that explain Red's design choices better over time, this page contains some of these questions and requests, which we can point people to. More features are planned for Red and, yes, we even look at modern features.

If you really, adamantly, deeply feel that Red *must* have your feature-of-choice or it is doomed to failure, and its value isn't immediately clear to the team (who will respond by slapping their collective foreheads and raining down RED tokens upon you), the onus is on you to prove its worth and viability as an integral part of the language. The burden is *not* on Team Red, or its designer to justify why it shouldn't be added. 

That said, some new features will be great, and we all want to improve Red. So dig in, do your homework, defend your position, arm yourself with objective facts and hard numbers ("too slow" and "much faster" are not numbers, in case you were wondering; nor is "scale" a unit of measure), and make it impossible for us to refute your claims about objective benefit. Then, and this is the hard part, make it clear that you understand Red and show how your new feature dovetails beautifully into its design and architecture. That's how you win.


# Table of Contents

1. [Immutability](#immutability)
1. [Lexical Scoping](#lexical-scoping)
1. [Variables](#variables)
1. [Strong Typing](#strong-typing)
1. [Case Sensitivity](#case-sensitivity)
1. [Zero Based Indexing](#zero-based-indexing)
1. [Operator Precedence](#operator-precedence)
1. [String Interpolation](#string-interpolation)
1. [Class-Based OOP](#class-based-oop)
1. [Use XXX Syntax](#use-xxx-syntax)
1. [Easier Custom Datatypes](#easier-custom-datatypes)
1. [Support Variable Arity Functions](#support-variable-arity-functions)
1. [Sandboxing](#sandboxing)
1. [JS as a Back End Target](#js-as-a-back-end-target)
1. [Sigils to denote destructive functions](#sigils-to-denote-destructive-functions)


# Immutability

# Lexical Scoping

# Variables

# Strong Typing

# Case Sensitivity

# Zero Based Indexing

# Operator Precedence

# String Interpolation

# Class-Based OOP

# Use XXX Syntax

# Easier Custom Datatypes

Adding new datatypes to Red must be done in Red/System, which is not terribly difficult, but it does mean dropping to a C level language. Beyond implementing the datatype itself, you also have to add it to the system, and update the lexers if you want your type to have a literal form. Red could add higher level features to make this easier, though the types you could create might be constrained by doing so, which means they will be more like existing types and add less unique value.

While making your own types is a great learning experience, keeping the foundation of Red consistent is important, because Red is about data exchange as much as execution. The more custom types people add, the harder it is to share it with others. Fortunately, it's rare that you would *need* to add a new datatype. Red lets you build almost anything at the user level (what we sometimes call the mezzanine level), which is what you should do first anyway, to try out ideas. If you have a custom literal form to try, `system/lexer/pre-load` is your friend. 

Consider, too, whether you just want tagged structures (blocks, objects, maps), which is easy to do with a convention and helper funcs. Custom datatypes lead to two possible approaches: 1) custom types are unique `datatype!`s, which means they MUST share all actions of other Red values, and not add any new ones (OO people won't like that), or they (`udt!`s) are a new datatype themselves, and the rest of Red has to accommodate them. That's a tough sell without a clear benefit.

@rebolek shows [here](http://red.qyz.cz/dependent-types.html) how easy it is to experiment with dependent types in Red. Or @maximvl's experiments with [dynamic variables](https://gist.github.com/maximvl/2095276ba6000c644d11049227176e68) and [Common Lisp condition restarts](https://gist.github.com/maximvl/dcb8c4e9ef5d4db91f7a6b52da9b9cee). @numberjay even played with [persistent immutable blocks](https://gist.github.com/numberjay/3df8f13044145c6dde1918ea2cdfe3b8).

Datatypes are a foundation of Red, as with other languages. But Red is quite different than, say, C++. Ask not whether you can add new types that look like native types, but ask yourself if you need to. Will the result truly be better that way. If so, maybe that's a candidate for a new type, if it's general enough. That's where talking to the community comes in. Some of them have used Redbol langs for decades, and have valuable instinct and experience. Look at Red/System as well. It's a dialect of Red.

# Support Variable Arity Functions

Red already has free ranging evaluation, refinements, and the ability to pass blocks of values. The first makes variable arity hard for users to scan, the other two are how you do it idiomatically in Red. If someone figures out how to make variable arity funcs clean, efficient, unambiguous, and easy to understand when used (consider more than C or Lisp style code), we'll hire them. But we'll have them work on other features. :^)

# Sandboxing

# JS as a Back End Target

It sounds appealing, on the surface, but the costs...oh, the costs. Targeting JS is not beyond the team's technical ability. That should be made very clear. But we have this mission, this goal, this purpose in life, to fight software complexity. As soon as we buy into that world, we lose a piece of our soul, and are then sitting on top of a big pile of...stuff. And people will use all those libs and frameworks, which are not designed to be anything like Red, so now we've just added to the polyglot stack of languages and concepts. So we shouldn't do it, because it will just hurt the poor web devs. ;^)

Red running on every possible platform would be a great thing. But being on those platforms won't make Red successful. It has to achieve a certain level of success first. So it's not that we are against this, it's just not a priority. It raises a lot of questions as well, because JS is not just an isolated language in practical use. It is the core of an ecosystem that branches out in many directions, from the DOM (and how View/VID translate there), to frameworks, to changes in the language, to node.js as a runtime.

But there's another approach: Write a Red interpreter in JS. Gabriele Santilli (@giesse) had his Topaz project to experiment with the idea, and @ALANVF has [Red.js](https://github.com/ALANVF/Red.js) in the works. This doesn't answer all the questions, but lets people look for answers and raises more. Remember that a big part of Red is the format, while JS has JSON, so it's a great way to explore ideas in a concrete context and see how things fit together.

# Sigils to denote destructive functions

This came up in chat, and led to some fun comments.

Q)
> Should Red adopt the lexical indicator"!" at the end of function names for destructive functions?
> A symbol at the end is an easy way to know at a glance.
> It's not just in the learning. It's also about the remembering. Neither are necessary if there is a lexical indicator at the end of a function name. It's self documenting.

A) No. `!` is used for datatype notation, and also because Red doesn't view mutability as dangerous. If we use a warning sigil it should be for other things, like "Boris wrote this func, so you can trust it to work but you may not understand the code." :^) Consider this, do you want to write `append!` every time, rather than `append`. What about `insert!, change!, replace!, sort!, remove-each!`? It's easy enough for anyone to alias these names if they want, and use them locally, which others can then see in action and discuss more. Other sigils could be used, to avoid the `datatype!` conflict, but the issue of extra noise and clarity remain. I'm a fan of sigils when used appropriately and in moderation, and they work incredibly well in Red's native forms, but that also makes lexical space tight for other purposes.

It's not so much a problem in programming, but in learning. And that runs to the core of Red and things like [why you have to copy series](https://github.com/red/red/wiki/%5BDOC%5D-Why-you-have-to-copy-series-values). There's just no avoiding this. We also have a standard practice of noting modifiers in doc strings, which is fallible of course, just like semantic versioning. But it's also easy to fix when we find one that isn't noted. If we get good coverage on that convention, then `? "modified"` in the console makes it easy to find them.

We could do this: `danger⚠: func [] []` or this `reverse☠: func ["DESTRUCTS YOUR SERIES!!" series..][..]`

Maybe that's how we deprecate functions. Rename the old ones based on why they were deprecated. Bug, crash, turtle for slow ones.

> The ruby way is to have a non destructibe version and a destructive version where appropriate. `remove!` and `remove`

Which comes back to the point that mutability is not the exception in Red, it's the norm. They then have 2 versions of each func for those cases. We don't want that. Other suggestions came in from the community as well, for how to address this issue:

> Use an editor that allows you to create your own group of keywords, and set it right color which suits your feel of danger.

> It's very easy to remember: Assume all functions mutate the input series. Add `copy` if you don't want that. If you need to optimize your code, then look at the source of the functions you are using and remove any redundant `copy`.

Side note, when it comes to how we address errors due to misuse of functions, @rebolek had the best answer:

> We can also make adaptive errors:
```
>> context [
    err-count: 0
    errors: [
        "x must be integer!"
        "you made the same error again, x must be integer!"
        "are you kidding me? I already told you that x must be integer!"
    ]
    set 'f func [x][
        unless integer? x [err-count: err-count + 1 do make error! pick errors err-count]
    ]
]

>> f "a"
*** User Error: "x must be integer!"
*** Where: do
*** Stack: f  

>> f "a"
*** User Error: "you made the same error again, x must be integer!"
*** Where: do
*** Stack: f  

>> f "a"
*** User Error: {are you kidding me? I already told you that x must be integer!}
*** Where: do
*** Stack: f
```
