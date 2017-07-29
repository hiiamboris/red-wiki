> When using `do`, do macros in multiple files affect previously "done" files?

Current macro support happens at the preprocessing stage (at final part of `load`). Macros are accumulated and applied globally. You can constrain that using the `#local` and `#reset` directives. Macros will most probably move at a later stage (becoming context-aware), once we get some assurance that it is safe to do so.

`Do` invokes the interpreter, so whatever you put after it, is not compiled. The compiler "equivalent" of `do` is the `#include` directive. `#include` is converted to `do` when the script is run from the interpreter, so that the symmetry is preserved.

The purpose of macros is to provide a compile-time optimizations. The only reason macros are there is to move costly computations from run-time to compile-time. What I mean by "costly" is computations that take enough time to be perceived by a human (anything less is not worth moving to macros), or that consumes huge memory chunks, or that require external resources which are not possible or expensive to use at run-time.

There is nothing macros can do that you can't do with regular code, with the same expressive power. Macro support in interpreted code is just for compatibility with the compiler.

For purely preprocessing source code, macros bring nothing that we don't already have. Maybe the only gain there, is a bit more familiar approach to people used to macro systems in other languages. Though that only applies to named macros, pattern-matching macros are just pure Parse rules. So, I suggest using macros in Red as an optimization tool (which is what they are good for), rather than the default go-to tool for source preprocessing (better use the core language facilities for that).