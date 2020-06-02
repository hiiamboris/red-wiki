(NB: Feel free to append new questions at the bottom.)

# The Name

Red is a descendant of Rebol. Rebol was originally all caps, as an acronym for Relative Expression-Based Object Language. It was pronounced rebel, not ree-ball, and people thought it was a COBOL like language. But it was unique, which was good for searches. Naming is hard.

The questions and comments we get most about the name are what it stands for, and that it's too common a word, so hard to get good search results. We call ourselves Reducers, and you can think of Red as a language with the goal of (red)ucing complexity. Or you can just think of it as a nice, short name. We have fun thinking of backronyms too of course. 

As to coming up with a unique name that's easier to search on, it's not a solution. You can search for "Red lang ..." or "Red language ..." to narrow results, just as you needed to for Go, Rust, and others before your tracking search engine learned what you really wanted. We are in good company, with Go, Rust, C, Ruby, Python, Swift, Elm, and others, but also appreciate unique names that designers have chosen for their creations.

# Technical

**Q. What do you mean by "full-stack language"? Isn't JS one?**

The Red tower-of-Hanoi [logo](http://i.stack.imgur.com/4iHfk.png) is a metaphor for the multiple layers of abstraction that Red aims at supporting, from hardware to highest abstractions, like DSL and meta-DSL. We use the term _full-stack language_ in this context to describe a programming tool able to cover all the abstraction layers. The "real" _full-stack programmers_ should be enabled to code at any abstraction level (through the vertical software layers stack) rather than the horizontal layer of the web applications. JavaScript primarily only covers the client- and server-side aspects (when a node.js backend is used).

**Q. What about using LLVM as backend?**

This option was considered at the beginning, but quickly discarded as it would be a bad fit for Red from several perspectives:

* Relying on LLVM, even if it has support for a broad range of features, would have limited the possible choices for Red's architecture and memory management options. 

* Red/System aims at providing full access to hardware features and be a good fit for building an operating system in the future.

* It requires to be able to statically link with LLVM libraries from the beginning and make the right functions and structures mappings. This would have taken more time than the direct path taken using a custom lightweight lower layer. (Note: Red has its own very small custom toolchain, it does not rely on Gcc or any other tool except for Rebol during the bootstrapping stage)

* LLVM adds several megabytes (5-10MB AFAICT) to the executable binary, this is something that we definitely want to avoid. We expect Red binary, with the JIT and Red/System compiler, to be in the 100-200KB range. You could think this is not relevant anymore, until you would need to upload your app to your favorite smartphone, pesting at how much time it takes and how much space it wastes, or trying to download such app through a low-band connection (not uncommon outside of the western world). 

* It is still possible to add LLVM as a target in the future, if this provides Red with a feature that we do not want or cannot afford to implement from scratch (unlikely, but not impossible).

**Q. Why don't you use Sphynx like in kernel.org instead of Asciidoc?**

Github supports Asciidoc and it appear that there's a bitbucket plugin for it too. The [LWN kernelorg article](https://lwn.net/Articles/692704/) prefers sphynx over asciidoc not for any better flexibility in sphynx's syntax but rather that it's written in python compared to asciidoctor's ruby and there doesn't already exist ruby code in the kernel tree. The article points out that asciidoc should be a better choice in the future, but sphynx is a better choice (for their needs) now. I think Red should stick with asciidoc. There hasn't been mention here of the Red documentation project suffering from a lack of functionality in asciidoctor's syntax or toolchain. Changing just for changing's sake is not wise.

# Why does `append` flatten its `value` argument?

For example, why does `append a [3 4]` produce `[1 2 3 4]` instead of `[1 2 [3 4]]`?

The default behavior of `append` (and other series actions) with regard to block arguments is useful because Red relies on fixed-arity functions. Specifying "several values" (to simulate variable arity) can only be done by passing a block (or a `any-block!` container, like `paren!`). Such usage is common enough to deserve to be the default behavior. When you want a container type to be treated as a single value, just use `append/only`. You'll see `/only` used with other functions as well.

# Why is the header case sensitive? That is, you have to use `Red`, not `red`.

`Red` followed by a block (square brackets) is used by the lexer to find the beginning of a Red script, ignoring everything before it. You have the same feature in Rebol. But Rebol, being a unique name, reduces the odds you will find it in random text followed square brackets. In Red's case, `red` is a common word, so the risk of false positives are higher. Capitalizing the first letter is a way to reduce that risk.

# How do I make it so that compiled GUI programs on Windows don't spawn a parent console on startup?

compile with `-t Windows`

# How can I make the compiler accept more dynamic code?

Code like this runs fine in the interpreter, but won't compile:

```red
direction: "south"
set to-word direction 4
probe south
== 4
```

```red
** script error: undefined word south
```

A compiler reads and tries to make sense of your code in advance, an interpreter figures it out while evaluating it.

In `probe south`, the `south` word is not defined explicitly. That is, the compiler does not see where it is defined; so it will signal an error. The compiler does a static check of the source code, while the interpreter processes the code on-the-fly. This way, the compiler can detect some simple errors and generate better code. In cases like this, you can either make the compiler happy by declaring `south` (like `south: none`), or disable compiler checks by adding the following entry inside your code's header: e.g. `Red [Config: [red-strict-check?: no]]`.

# How can I load non-UTF-8 strings in Red

https://stackoverflow.com/questions/43379932/access-error-invalid-utf-8-encoding-ffd8ffe0/43383454#43383454

