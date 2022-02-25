Compiler overview: http://www.red-lang.org/2011/05/redsystem-compiler-overview.html (old)

Objects Implementation Notes: http://www.red-lang.org/2014/12/objects-implementation-notes.html

GET of compiled object method doesn't run: [#924](https://github.com/red/red/issues/924)

Inner func capture compiler issue: [#2542](https://github.com/red/red/issues/2542)

Returned value from function defined in context differs during compiled app execution: [#2910](https://github.com/red/red/issues/2910)

`Return` inside `try`: [#5086](https://github.com/red/red/issues/5086)

When compiling mixed Red + Red/system programs, it is necessary to compile using the release mode option (-r).

[#1342](https://github.com/red/red/issues/1342) is a dynamic limitation. The compiler does not support such constructs, nor will it support them in the near future (written in mid 2018). The alternative is to use the encapping mode when compiling (-e option instead of -c), that will ensure that your code runs through the interpreter, fully preserving the semantics you experience in the REPL.

Additional issues for compiler limitation: [#3285](https://github.com/red/red/issues/3285) [#1748](https://github.com/red/red/issues/1748) [#1977](https://github.com/red/red/issues/1977)

Another dynamic limitation example:

```
Red []
b: [x y]
f: func b [print x + y]
f 3 4
```

When compiled and executed this code, it doesn't print anything. Use `-e` (encapped mode)

# Construct

https://github.com/red/red/issues/4815

# Self

https://github.com/red/red/issues/5018

# Function assignment chaining

See: https://github.com/red/red/issues/3862

`single?: last?: func [`

The way the single? function is defined is not compatible with the compiler. The compiler processes runtime source files specifically to extract function definitions, and currently only recognizes `name: func` and `name: :other-name` patterns for defining functions.