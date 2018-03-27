Compiler overview: http://www.red-lang.org/2011/05/redsystem-compiler-overview.html (old)

Objects Implementation Notes: http://www.red-lang.org/2014/12/objects-implementation-notes.html

GET of compiled object method doesn't run: [#924](https://github.com/red/red/issues/924)

Inner func capture compiler issue: [#2542](https://github.com/red/red/issues/2542)

Returned value from function defined in context differs during compiled app execution: [#2910](https://github.com/red/red/issues/2910)

When compiling mixed Red + Red/system programs, it is necessary to compile using the release mode option (-r).

#1342 is a dynamic limitation. The compiler does not support such constructs, nor will it support them in the near future (written in mid 2018). The alternative is to use the encapping mode when compiling (-e option instead of -c), that will ensure that your code runs through the interpreter, fully preserving the semantics you experience in the REPL.

