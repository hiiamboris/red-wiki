People coming to Red from other languages often suggest that Red should adopt features from that language. The problem is that a language is not just a collection of features. You can't just drop new ones in and make the language better. You have to consider the language as a whole, its goals and aesthetics; its foundational principles and the conceptual integrity as viewed through different lenses. That doesn't mean Red can't adopt new features, just that it generally isn't quick and easy. Features make languages different, but that's not the same thing as making them better.

As language trends change, certain feature requests will come up again and again, repeatedly, over and over. To avoid endless chat that changes only slightly on each iteration, and to collect thoughts that explain Red's design choices better over time, this page contains some of these questions and requests, which we can point people to. More features are planned for Red and, yes, we even look at modern features.

If you really, adamantly, deeply feel that Red *must* have your feature-of-choice or it is doomed to failure, and its value isn't immediately clear to the team (who will respond by slapping their collective foreheads and raining down RED tokens upon you), the onus is on you to prove its worth and viability as an integral part of the language. The burden is *not* on Team Red, or its designer to justify why it shouldn't be added. 

That said, some new features will be great, and we all want to improve Red. So dig in, do your homework, defend your position, arm yourself with objective facts and hard numbers ("too slow" and "much faster" are not numbers, in case you were wondering), and make it impossible for us to refute your claims about objective benefit. Then, and this is the hard part, make it clear that you understand Red and show how your new feature dovetails beautifully into its design and architecture. That's how you win.


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

Red already has free ranging evaluation, refinements, and the ability to pass blocks of values. If someone figures out how to make variable arity funcs clean, efficient, and easy to understand when used (consider more than C or Lisp style code), we'll hire them. But we'll have them work on other features. :^)

# Sandboxing

# JS as a Back End Target

It sounds appealing, on the surface, but the costs...oh, the costs. Targeting JS is not beyond the team's technical ability. That should be made very clear. But we have this mission, this goal, this purpose in life, to fight software complexity. As soon as we buy into that world, we lose a piece of our soul, and are then sitting on top of a big pile of...stuff. And people will use all those libs and frameworks, which are not designed to be anything like Red, so now we've just added to the polyglot stack of languages and concepts. So we shouldn't do it, because it will just hurt the poor web devs. ;^)

Red running on every possible platform would be a great thing. But being on those platforms won't make Red successful. It has to achieve a certain level of success first. So it's not that we are against this, it's just not a priority. It raises a lot of questions as well, because JS is not just an isolated language in practical use. It is the core of an ecosystem that branches out in many directions, from the DOM (and how View/VID translate there), to frameworks, to changes in the language, to node.js as a runtime.

But there's another approach: Write a Red interpreter in JS. Gabriele Santilli (@giesse) had his Topaz project to experiment with the idea, and @ALANVF has [Red.js](https://github.com/ALANVF/Red.js) in the works. This doesn't answer all the questions, but lets people look for answers and raises more. Remember that a big part of Red is the format, while JS has JSON, so it's a great way to explore ideas in a concrete context and see how things fit together.

# Sigils to denote destructive functions


@gltewalt:matrix.org

Should Red adopt the lexical indicator"!" at the end of function names for destructive functions?

----

@hiiamboris

No, it would conflict with datatypes. I like this idea actually, since I first seen it in some other lang. But that means no compatibility with Rebol.

----

@greggirwin

@gltewalt:matrix.org no. ! is used for datatype notation.
And because we don't view mutability as dangerous, if we use a warning sigil it should be for other things, like "Boris wrote this func, so you can trust it to work but you may not understand the code." :^)

----

@hiiamboris

:D

----

@gltewalt:matrix.org

A symbol at the end is an easy way to know at a glance. Use the Irwin smiley face

----

@greggirwin

Or we could do this: danger⚠: func [] []
I'm a fan of sigils when used appropriately and in moderation.

----

@hiiamboris

reverse☠: func ["DESTRUCTS YOUR SERIES!!1" series..][..]
Seriously though, I find that one gets used pretty quickly to which functions are modifying. It's not a problem in programming, only in learning.

----

@greggirwin

That's how we deprecate functions. Rename the old ones based on why they were deprecated. Bug, crash, turtle for slow ones.
And we have a standard practice of noting modifiers in doc strings.
There's nothing preventing people from doing that kind of thing in their own libs of course. It may be very effective in some cases.

----

@gltewalt:matrix.org

It's not just in the learning. It's also about the remembering. Neither are necessary if there is a lexical indicator at the end of a function name. Self documenting.
borisize*

----
@hiiamboris

;) I would have supported the initiative if not for Rebol roots.

----

@greggirwin

@gltewalt:matrix.org propose a different sigil then. Red won't use ! to denote mutability without very strong arguments behind it. Even better, evidence that it helps.

----

@gltewalt

I just did. *

There are no great other ascii symbols, though
People may be used to * for pointers

----

@greggirwin

borisize* v * n* vdoesn't look great to me. Good point on pointers. Lexical space is tight, so it's tough.

Consider this, do you want to write append! every time, rather than append. What about insert!, change!, replace!, sort!, remove-each!? Modify some code and see what it looks like if you denote all current modifying funcs this way. It would be interesting to see.
Thanks for the link to the construct issue. I'll note that as well.

----

@gltewalt

Yeah that might get kind of noisy if you have a big chunk of code that contains a bunch of modifiers
The ruby way is to have a non destructibe version and a destructive version where appropriate. remove! and remove

----

@greggirwin
Which comes back to the point that mutability is not the exception in Red, it's the norm.

----

@gltewalt

Well it's not exactly the exception in ruby either
Just thought I'd throw it out there and see what others think.

----

@greggirwin

And they have 2 versions of each func for those cases, right? We don't want that.

----

@gltewalt

Yes
Can we at least get Irwin smiley face in certain error messages?

----

@greggirwin

@gltewalt could I impose on you to add a section, and notes from this chat to https://github.com/red/red/wiki/%5BDOC%5D-Red-Should...-(Feature-Wars) ?

----

@Oldes

@gltewalt maybe you should just use some editor which allows you to create own group of key-words and set it right color which suits your feel of danger.

----

@giesse

@gltewalt it's very easy to remember.
Assume all functions mutate the input series. Add copy if you don't want that.
If you need to optimize your code, then look at the source of the functions you are using and remove any redundant copy.

----

@gltewalt

It's not about my feel for what is dangerous, its a general inquiry and proposal to see what red users think about it. Or if they think about it.

----

@greggirwin

@gltewalt we can add smileys to error messages when it's the user's fault, but we don't want them to feel too bad about it. :^)

----

@rebolek

We can also make adaptive errors:

>> context [err-count: 0 errors: ["x must be integer!" "you made the same error again, x must be integer!" "are you kidding me? I already told you that x must be integer!"] set 'f func [x][unless integer? x [err-count: err-count + 1 do make error! pick errors err-count]]]

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

