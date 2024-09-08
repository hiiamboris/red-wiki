_Asked by greenmughal on [gitter](https://rebol.tech/gitter.im/red/red/2019/#msg5cfbaa57bf4cbd167c4fa040)_

> How to build an interpreter for Red lang, for learning purpose, where I can start?

_Answered by 9214_

Start by [learning the language itself](https://github.com/red/red/wiki/[LINKS]-Learning-resources) and understanding its fundamental pieces: values, datatypes, series, binding, homoiconicity, load and do phases, dialects; even though Red has some similarities with Lisp family, its design is fairly unique and deserves a thorough study.

Then dig through the sources and learn about internal data representation, cell layout, memory allocation, datatypes API, etc. I recommend to start with early versions (e.g. [0.3.2](https://github.com/red/red/tree/v0.3.2/red), which contains the first version of interpreter), for simplicity.

Aim at a minimal subset, say, `integer!`, `word!`, `block!` and `object!` datatypes and a few natives for control flow with arithmetic/logic operations, no garbage collection, ASCII encoding. To implement a robust lexer, I'd recommend to learn Parse dialect and study [lexical scanner](https://github.com/red/red/blob/master/environment/lexer.red) in the main branch. If you prefer C, then study [Rebol3 codebase](https://github.com/rebol/rebol).

If you have an advanced knowledge of Lisp, you can also study the first version of [Rebol compiler](https://github.com/akavel/sherman), written in Scheme.