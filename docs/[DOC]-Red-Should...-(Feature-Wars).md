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
1. []()


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
