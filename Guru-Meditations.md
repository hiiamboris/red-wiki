# Debugging

@SteeveGit hit a crashing bug under 0.6.0: `view [button "crash" [type?/word event]]`

It led to issue [#1983](https://github.com/red/red/issues/1983). You should read the full guru answer there.

tl;dr is that you should compile with `-d` for debug mode, and that simple tools (`probe` `??` `dump4` `stack/dump` `print-symbol`) are generally enough. Remember that Red's toolchain is minimal, building from source is fast and easy, and the source base is small and well organized. And if you've only ever worked at the Red level, a small amount of time learning some Red/System can go a long way. Even if you aren't confident you could write a fix, you may help save others time, and have fun exploring in the process. Don't be afraid.

# Red/System Boxing

Boxing is the way to transform primitive R/S values into Red values, which are stored in 128-bit slots. The %runtime/datatype/structure.reds file gives you the slot layout for every Red datatype.

Every datatype has a way to "box" a primitive value into the higher-level type. The "/box" functions, which are present in some datatypes (but not all) give you a simple way to achieve it. Other types use /make-in or /make-at, but they are lower-level and require you to provide the destination slot pointer as argument.

There are also the /push functions (mostly used by the compiler), which are handy, but they will pile up the boxed value on the stack, while the /box functions will store it at stack/arguments, which represents the first stack entry of the current function call frame. That slot is also used for the returned value, so you'll see it used extensively pretty much everywhere in the Red runtime code.
 
# Decorated Native Names

Some natives have a `*` decoration added to their names. It's used when the word could conflict with an existing R/S definition, in such case, a decorated/undecorated pair needs to be added to `process-typecheck-directive` in %compiler.r.

# Interrupting Tests

If you interrupt the testing framework before it completes, you need to manually kill the remaining tasks before starting it again.

# REBOL version during bootstrap phase

If you're running Red tests and building Red from source, you may want to use 2.7.6 instead of 2.7.8. 2.7.8 has some `call` problems that the Red team tried to work around, but may still hang.

# Red compiler and dialects

As of 0.6.0, the Red compiler can't process dialects. The interpreter doesn't have this issue, and the compiler will, correctly, complain.

```
quit?:  none
canvas: none    ;<== Add this to get it to compile

win: layout/tight [
    size win-size
    origin 0x0
    canvas: base win-size 10.10.255 draw draw-blk
]
win/actors: context [
	on-close:    ACTOR [quit?: yes]
	on-resize:   ACTOR [] ;@@ Do what you want when face resized
	on-resizing: ACTOR [] ;@@ Do what you want when face resizing
]
view/flags/no-wait win [resize]
until [
    ;@@ Update canvas draw block
    show canvas
    do-events/no-wait
    quit?
]
```

# Reactive Cycles

Reactions are pushed on a stack just before they are evaluated, so the engine can detect cycles in chained reactions and break them before they loop. So, it can update all the nodes in "graph D" without looping, regardless of which node triggers the change.

# Define infix operators

To create an infix function, you first need to make a prefix function with 2 arguments. Then you make an `op!` from that function.

```
make-range: function [a [integer!] b [integer!]][
    collect [i: a - 1 until [keep i: i + 1 i = b]]
]
->: make op! :make-range

2 -> 7
== [2 3 4 5 6 7]
```

You can use any valid word as the the infix function name.

# Hide the cmd window under Windows, when running %.red scripts

*From Steeve*

*This will likely change as Red matures.*

```
console: system/view/screens/1/pane/-1
console/visible?: false
```

The console is hidden, but still running. If you don't use `quit` to shut down your app, the attached cmd process is still alive. 

To shut down properly when the user clicks the close box [x] on the window, you can use the `on-close` actor:

```
view/options [text "hi"][
    actors: object [ on-close: func [face event][quit] ]
]
```

# Getting output in a shell when running a Red script

Use `red --cli <scriptname>` to redirect output to the shell. By default, Red uses its own GUI console, not the system shell. `--cli` forces it to use the system shell.


# Exposing Red/System macros in Red

> I have some macro constants defined in Red/System. How can I reuse them in Red without redefining them again, in Red? 

You can't re-use macros. They get inlined and disappear during compilation. Maybe, with some clever code, you could create a literal array containing your macros constants and have a function that converts them to a `red-block!` on startup. Then you expose that to Red. You would still need to write and maintain that literal array of macros manually (but you could put them close to your macros definitions in R/S).


# Modifying data before `load`ing it (Lisp reader macros)

While you will normally use loadable Red data, there may be times where modifying the data before it's loaded will help. In Lisp, these are called *reader macros*. In Red, the way to do this is to patch the `load` function's body with logic that transforms the incoming data to the form you want when loaded. It may turn non-loadable data into loadable data, but it may also transform loadable data somehow.

**Needs to be updated for recent builds.**

```
pre-load: func [src part][
    parse src [any [remove comma insert #" " | skip]]
]
load: func spec-of :load body: body-of :load
insert find body 'case bind [pre-load source part] :load

[1,2,3,abd,"hello"]
```

# Compiled versus interpreted behaviors

From @SteeveGit, for fun, based on chat about a Factor-like `fry` func, produced a nice example. It shows how you can replace a compiled func with an interpreted one, to change certain behaviors.

`Fry` using replace. Funny attempt. Replace can take the result of a function as substitution value. So in theory we could write.
```
fry: func [in out] [replace/all copy out '_ does [take in]]
```
Problem:
```
red>> fry [a b c][_ + _ - _ ]
= [func [][take in] + func [][take in] - func [][take in]]
```
The function is not evaluated at each iteration as hoped (like Rebol). The reason is that Red uses a compiled version of the replace function. Compiled functions don't evaluate functions passed as arguments inside their body code, like interpreted functions do. Reconstructing replace from its source "activates" interpreted mode.
```
replace: func spec-of :replace body-of :replace
```
Now, we get the expected behavior:
```
red>> fry [a b c][_ + _ - _]
== [a + b - c]
```

# Keeping the CLI console open

> My test.red has only one line halt. When I execute `red.exe --cli test.red` output is "(halted)" but the console closes. If I execute the Red GUI console it stays open as expected.

Add `--catch` on the command-line, it will force the console to stay open, even on errors. It's designed that way so you can run Red scripts through the interpreter from shell scripts, without being annoyed by the console staying open on errors. The GUI console lives in a different environment, where it needs to stay open, otherwise you can't see error messages.

# Cloning Objects

`Make` clones referenced series (e.g., block! is a series) in an object, because this behavior best covers the common use-cases. Objects and maps are heavy datatypes. You most often want to keep a reference to those, not clone them. You can clone them manually using copy when you really need it. Trees of deep-copied objects or maps come from trying to bend the language to work in a purely OOP way. If Red you should model such trees using blocks.

# libRed 

As of 10-Oct-2016 the libRed branch has been merged to Master. The toolchain now behaves differently. Here is a brief overview:
- Compilation of Red scripts now has two modes: development and release.

- Development mode ("dev mode") is the default. Release mode is achieved with an extra `-r` or `--release` compilation option.

- Dev mode first builds the Red runtime as a shared library (libRedRT) and then reuses it across compilations. It puts it in the working folder, along with some other temporary libRedRT* files. You can freely delete them at any time.

- Dev mode provides much faster compilation, by compiling only user code and not the Red runtime library.
If your Red script contains R/S code and relies on the Red runtime API, you should compile once with the `-u` option, to force a custom update of libRedRT (which takes a bit of time), then you can compile as usual and enjoy fast compilation.

- Pure R/S scripts are unaffected by those changes, so they are handled as usual.

- There are still some details to work on, but it is very usable already.

Notes:

LibRedRT is only for Red's runtime code + modules. Currently the list of modules is fixed, but it will be possible to set it manually soon. On Windows, you get View module precompiled in libRedRT.

If you want to get back to old toolchain behavior, just specify `-r` on command-line.

The Red GUI console can only be compiled in release mode (`-r`) right now. It cannot use libRedRT, due to some tight coupling with the runtime library (printing functions mutual dependencies).

# How to reference the current View container

`Self` references the current face container from do blocks in `view`:
```
view [
    f1: field on-enter [face/parent/selected: f2]
    f2: field on-enter [face/parent/selected: f3]
    f3: field
    do [self/selected: f1]
]
```
You can also set a word to that self reference for later use:
```
view [
    f1: field on-enter [win/selected: f2]
    f2: field on-enter [win/selected: f3]
    f3: field
    do [win: self win/selected: f1]
]
```