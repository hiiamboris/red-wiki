### Table of Contents

1. [Overview](#overview)
1. [What's in the name?](#whats-in-the-name)
1. [Why renaming is a bad idea?](#why-renaming-is-a-bad-idea)
1. [How can I improve Red's visibility?](#how-can-i-improve-reds-visibility)

### Overview

Red's history can be traced from one [visionary idea](https://youtu.be/-KqNO_sDqm4) to a diverse community that values simplicity and embodies a culture of taking the stance against software complexity. As we keep IT simple time and again, questions about the project's popularity, visibility, and user onboarding start posing themselves, which prompts one to reflect on the ways we can make the public more aware of our efforts. Naturally, people turn to Red's name and branding, questioning whether it can be changed so as to improve searchability and communication of the project's values and goals.  

At this time and place, however, such renaming proposals do more harm than good, even if said in jest. The aim of this article is to elaborate on why it is so, and to propose better ways of addressing the underlying concerns. But first, a little bit of history.

### What's in the name?

> The named is the mother of ten thousand things.
> <br> **— Lao Tzu**

The folk legend says that Red, as a name, was conceived as an acronym for Rebol's Extended Dialect. While it is true that Red carries the torch lit by Rebol, the etymology itself is after-the-fact theorizing and abductive reasoning: REBOL was an acronym (Relative Expression Based Object Language), so as its conceptual predecessor LAVA (Language for Audio and Video Applications).

The actual origin of the name (that we take up as an authoritative) is that Red is a shortcut for "reduce", or rather it is a _reduced_ version of "reduce". That makes Red both context-sensitive and [autological](https://en.wikipedia.org/wiki/Autological_word) word, which reflects language's core semantic concepts and mechanisms (such as binding, series model, homoiconicity, and meta-circularity) and makes itself into the "Reduce software complexity" motto.

```red
>> block
== [red red]
>> name: get block/1
== "uce"
>> head name
== "reduce"
>> set block/1 take/part head name name
== "red"
>> block: reduce block
== ["red" 255.0.0]
>> view [base with [color: last block]]
```

It does not end here, however. Red-the-language is not a thing in itself, but a foothold for other dialects and language environments that can be built on top of it, thanks to a shared data format: names such as Red/System, Red/View or Red/C3 all take their root in Red, and, in a way, refine Red's semantics, narrowing/reducing it down to a domain-specific purpose.

```red
>> type? /system
== refinement!
```

### Why renaming is a bad idea?

> A rose by any other name would smell as sweet.
> <br> **— Shakespeare**

Now to the main point: why changing the name to something else is viewed as detrimental?

First and foremost, it hurts project's identity; it could have made sense to come up with a different name at nascent stages of development, but now, when Red slowly but steadily starts to gain traction and has quite a bit of story behind it, that would only cause confusion and diminish media coverage area, rendering all previous mentions of the project obsolete.

Secondly, it's not that simple, and would require a total re-branding. As Red matures, it spawns an ecosystem of companies and corporate entities that support and back it up, such as Red Foundation or Redlake Technologies. Establishing a commercial arm of the project is a no small feat, and renaming it mid-way is a thorny path potentially leading to trademark issues with entrepreneurial red-tape (no pun intended).

Lastly, such renaming does not really address the main problem it was designed to solve: searchability and public awareness. The most common complaint about Red's name is that it's too common and can be easily confused with a color — that is correct, but entirely misses the point. **\***

Taken in the context of software, Go is not an ancient board game, Rust is not an iron oxid, Python and Pony are not animals, C is not a letter, and C# is not a musical note. What differentiates these (and many others) programming languages is not their name, but the critical mass they managed to achieve in the course of their development. As it was said earlier, renaming won't help Red to achieve this mass, and, in fact, might even diminish it. **†**

And this brings us to the last point: how this critical mass can be achieved.

### How can I improve Red's visibility?

> Therefore, send not to know for whom the bell tolls, it tolls for thee.
> <br> **— John Donne**

Taken at the face value, the issue with searchability can be addressed by a dedicated initiative at Search Engine Optimizations (SEO): the spectrum of solutions ranges from technical ones, such as crawling, indexing, and ranking, to those that involve soft skills, such as word of mouth, inspiring tweetstorms, blogging, software evangelism on technical conferences, and marketing in general. **‡**

Though, more often than not, this problem tends to be over-exaggerated and can be easily solved by learning how to use your preferred search engine, or by using a specific set of resources as a starting point for asking Red-related questions: this [wiki](https://github.com/red/red/wiki), [Stack Overflow](https://stackoverflow.com/questions/tagged/red), official [documentation](https://doc.red-lang.org/) and [project's website](https://red-lang.org/), [community-provided content](https://github.com/red/red/wiki/%5BLINKS%5D-Learning-resources), and [Gitter chat](https://gitter.im/red/home).

All of that, however, will be all talk and not walk if there're no Red-powered technologies to speak about: what tips the scales the most is the value that Red brings to the world via developers and contributors that create something with it or improve the language itself.

You can be such a developer/contributor too, and, thanks to the project's scope and ambitions, there's never a shortage of tasks to cross out from a to-do list, and always a need for champions ready to sink their teeth into exciting problems: GUI backends, potential libRed embedding targets, open-sourced and commercial projects, dialect design, interop with other language's runtimes, writing documentation... and wiki articles.

---

**\*** Interestingly, red is one of the 3 universal [color terms](https://en.wikipedia.org/wiki/Basic_Color_Terms:_Their_Universality_and_Evolution), among black and white. This fact ties relative expressions to [linguistic relativity](https://en.wikipedia.org/wiki/Linguistic_relativity_and_the_color_naming_debate), and strengthens the idea of Red being a "primordial" base for other languages that it can embed (dialects) and be embedded in (libRed).

**†** This is by no means a general rule. [Perl 6 → Raku renaming](https://github.com/Raku/problem-solving/issues/81), for example, was an intentional casting aside of historical bonds, and aimed to improve both Perl 5 and Perl 6 project identities.

**‡** Though, keep in mind that any such activity needs an accurate timing, so that we don't repel the newly-attracted users by being not-quite-there-yet.