(NB: Feel free to append new questions at the bottom.)

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

# Why does `append` flatten its `value` argument?

For example, why does `append a [3 4]` produce `[1 2 3 4]` instead of `[1 2 [3 4]]`?

The default behavior of `append` (and other series actions) with regard to block arguments is useful because Red relies on fixed-arity functions. Specifying "several values" (to simulate variable arity) can only be done by passing a block (or a `any-block!` container, like `paren!`). Such usage is common enough to deserve to be the default behavior. When you want a container type to be treated as a single value, just use `append/only`. You'll see `/only` used with other functions as well.

# Why is the header case sensitive? That is, you have to use `Red`, not `red`.

`Red` followed by a block (square brackets) is used by the lexer to find the beginning of a Red script, ignoring everything before it. You have the same feature in Rebol. But Rebol, being a unique name, reduces the odds you will find it in random text followed square brackets. In Red's case, `red` is a common word, so the risk of false positives are higher. Capitalizing the first letter is a way to reduce that risk.

# How do I make it so that compiled GUI programs on Windows don't spawn a parent console on startup?

compile with `-t Windows`

