# Debugging

@SteeveGit hit a crashing bug under 0.6.0: `view [button "crash" [type?/word event]]`

It led to issue [#1983](https://github.com/red/red/issues/1983). You should read the full guru answer there.

tl;dr is that you should compile with `-d` for debug mode, and that simple tools (`probe` `??` `dump4` `stack/dump` `print-symbol`) are generally enough. Remember that Red's toolchain is minimal, building from source is fast and easy, and the source base is small and well organized. And if you've only ever worked at the Red level, a small amount of time learning some Red/System can go a long way. Even if you aren't confident you could write a fix, you may help save others time, and have fun exploring in the process. Don't be afraid.

# Red/System Boxing

Boxing is the way to transform primitive R/S values into Red values, which are stored in 128-bit slots. The %runtime/datatype/structures.reds file gives you the slot layout for every Red datatype.

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

While you will normally use loadable Red data, there may be times where modifying the data before it's loaded will help. In Lisp, these are called *reader macros*. Red has macros now as well, but if your needs are simple, you can use the `lexer/pre-load` feature to do it:

```
; Remove commas
system/lexer/pre-load: func [src part][
    parse src [any [remove comma insert #" " | skip]]
]

>> [1,2,3,abd,"hello"]
== [1 2 3 abd "hello"]
```
```
; French-enabled console:
system/lexer/pre-load: function [src part][
    parse src [
        any [
            s: [
                "affiche"         (new: "print")
                | "si"              (new: "if")
                | "tant que"    (new: "while")
                | "pair?"        (new: "even?")
                | "impair?"        (new: "odd?")
            ] e: (s: change/part s new e) :s
            | skip
        ]
    ]
]

i: 10 tant que [i > 0][si impair? i [affiche i] i: i - 1]
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

# libRed and libRedRT

libRedRT is the externalization of the Red runtime library, allowing other tools and languages to call into it (see: https://doc.red-lang.org/en/libred.html), and avoiding the cost of recompiling the runtime library each time. Much faster compilation and very small executables are a result. This compilation model is the default "development" mode, a "release" mode (`-r` or `--release` on the command line) compiles and links everything together as usual.

Overview:

- Compilation of Red scripts has two modes: development and release.

- Development mode ("dev mode") is the default. Release mode is used with the `-r` or `--release` compilation options.

- Dev mode first builds the Red runtime as a shared library (libRedRT) and then reuses it across compilations. It puts it in the working folder, along with some other temporary libRedRT* files. You can freely delete them at any time.

- Dev mode provides much faster compilation, by compiling only user code and not the Red runtime library.
If your Red script contains R/S code and relies on the Red runtime API, you should compile once with the `-u` option, to force a custom update of libRedRT (which takes a bit of time), then you can compile as usual and enjoy fast compilation.

- Pure R/S scripts are unaffected by those changes, so they are handled as usual.

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
Bonus information: `event/window` gives you a reference to the current window face in all event handlers.

# Compiling with the debug flag

> If I compile with the debug flag how can I make use of it?

The debugging compilation mode is currently limited to Red/System (as of Oct 2016). When enabled, you get stack traces and more detailed error info on crashing. It also enables assert expressions and additional debugging functions for Red/System code: `dump4`, `dump1` or `hex-dump` for various hex dumps. Moreover, it also enables extra verbose outputs, for parts of the runtime where verbose is set to a value > 0 (e.g. for [interpreter](https://github.com/red/red/blob/master/runtime/interpreter.reds#L88), or [parse](https://github.com/red/red/blob/master/runtime/parse.reds#L14)). You can search in the Red source code for `#if debug?` pattern to see all the features unlocked by debugging mode.

# symbol/resolve and val/symbol (in Red/System)

> What's the difference between `val/symbol` and `symbol/resolve val/symbol`?

`Word!` in Red is case insensitive. The word/symbol is the unique ID of a word, and calling `symbol/resolve word/symbol` we get the alias ID (the original ID) of a word. For example, we create a new word `abc`, it gets an ID, let's say 1000. Here `abc/symbol = 1000`. Then we create another word `ABC`, it also gets an ID, maybe 1001. So `ABC/symbol = 1001` because it's an alias of word `abc`, `symbol/resolve val/symbol` will get the alias ID which is 1000.

If you don't want to do strict comparison, use `symbol/resolve val/symbol`.

# Global vars in Red /System routines

> When using routines is there a way to have 'global' variables? Accessible between routines or between functions within a routine?

Use `#system` if you want your code to be under the internal Red namespace, or `#system-global` otherwise.

# Using Red/System contexts in Red routines

You may write apps that are a combination of Red and Red/System code. They can be combined in various ways. For example, let's say you want to write use Red/System code inside a Red app. You can write a `routine`. The routine body is written in Red/System, not Red, and is able to access Red/System contexts (e.g. `_random`). You don't have to include those separately, as they are already included in the runtime.

Rather than referencing them in a routine, you can put them in `#system-global`. If you do, you MUST prefix them with `red/` because the runtime library is enclosed in the `red/` context. You could also use a `#system` directive, which will place your code under the `red/` context for you.

# What is the format for the `request-file` `/filter` refinement?

```
[
    description-1 file-extensions-1
    description-2 file-extensions-2
    ......
]
description: (string!) Describes the filter (for example, "Text Files")
file-extensions: (string!) Specifies the filter pattern (for example, "*.TXT"). 
To specify multiple filter patterns for a single display string, 
use a semicolon to separate the patterns (for example, "*.TXT;*.DOC;*.BAK")
```

For example:
```
request-file/filter [
    "All Pictures Files" "*.jpg;*.png;*.gif"
    "Red Source Files (red;reds)" "*.red;*.reds"
]
```

# Red/System native! values, #typecheck, and macros

> In process-typecheck-directive in the compiler, certain built-ins are protected from macro replacement: unless, forever, does, prin, positive?, negative?, max, min. Why just these?

Because the compiler needs them untouched in order to identify the function's name. In the future the R/S compiler could know the function's name, and pass it through the #typecheck directive. Only native! functions are use #typecheck directives and, among those, only the few above collide with existing macros.

# Calling Red from Red/System

You can't use arbitrary Red code in a #call directive, e.g., `#call [make image! compose [(size) 255.255.255.255]]`. You can only call a single Red function, passing Red value pointers as arguments.

# How do I get the address of a struct! member as a pointer! type?

Red/System doesn't have that feature yet. Currently, you need to do manual pointer arithmetic to get the address.

# Why are contexts static?

If you could remove words from contexts, bound words could refer to contexts where they are not defined anymore. For example:
```
obj: context [a: 2 b: [a + 1]]
```
If you remove `a` from that obj context, `[a + 1]` could not be evaluated. It would crash, unless you manually rebound `a` to another context.

Moreover, the position of a word in the context symbol table is widely used internally, in both Red and Rebol implementations, so it would not be feasible to remove that "slot". In other words, you could "delete" the word from the object but the entry in the object's symbol table needs to stay or everything would collapse.

For adding words, you can do that already using `make` to create a new, extended object. If you need a data structure where you can freely add/remove key/value pairs, object! is not the right type. Block!, map! or hash! are much better suited for that.

# (GUI) Getting the tab-index of the selected tab inside a tab-panel

In `on-change`, the `face/selected` property contains the previously selected tab index and `event/picked` contains the newly selected tab index. If the previous and new tab-index are the same, `event/picked` is 0.

# Call libRed from C (Mac OS) and Ruby (Unbuntu32)

> Credit to Peter WA Wood

As libRed is 32-bit, you must use gcc to compile the program so it can call libRed. 

`red-in-c.c`
```
#include "<path to>/libRed/red.h"

int main(int argc, const char * argv[]) {
    redOpen();
    redDo("print {Hello Red}");
    redClose();
    return 0;
}
```
Here's the command to compile it:
```
$ gcc -m32 <path to>/libRed.dylib -o red-in-c red-in-c.c
```

Here's an example of calling libRed from Ruby using the FFI Gem.

```
require 'ffi'

module RubyRed
  extend FFI::Library
  ffi_lib "./libRed.so"
  attach_function :redOpen, [], :void
  attach_function :redClose, [], :void
  attach_function :redDo, [ :string ], :pointer
end

RubyRed::redOpen
RubyRed::redDo "print {Hello Ruby, I'm Red}"
RubyRed::redClose
```

# Why is Red 32-bit only?

Red is still in the alpha stage (as of early 2017). 64-bit support will come, but it will probably happen after 1.0 because of the effort needed. For 64-bit support, Red needs:

1. A 64-bit emitter for x64 and ARM64.
2. Support for 64-bit integers and pointers in Red/System.
3. Changes to the value slots, which are 128 bits currently, to cope with 64-bit pointers.
4. 64-bit executable file format support.

1 & 4 would be trashed once we start working on Red 2.0, so it's not worth investing time in that right now. 2 could be partially implemented before 1.0. 3 requires significant changes in the runtime code, and an elegant solution to handle both 32 and 64-bit models within the same codebase.

# PARSE COLLECT/KEEP options combined with TO/THRU rules

- `keep` by itself collects a single value or a list of values in range-capturing mode
- `keep pick` collects all the matched values separately
- `keep copy <word>` collects all the matched values as a series (of same type as the input)

For example:
```
red>> parse [x -- ] [collect [keep to '-- ]]
== [x]
red>> parse [x y -- ] [collect [keep to '-- ]]
== [[x y]]

red>> parse [x -- ] [collect [keep pick to '-- ]]
== [x] 
red>> parse [x y -- ] [collect [keep pick to '-- ]]
== [x y]

red>> parse [x -- ] [collect [keep copy _ to '-- ]]
== [[x]]
red>> parse [x y -- ] [collect [keep copy _ to '-- ]]
== [[x y]]
```

> Note: `_` is just a word. Any word would do. Here the underscore signifies that we don't care about that word or its value, as it is immediately collected by keep.

# Bind, In, Contexts, and Words versus Symbols

`Bind` or `in` rebind the argument value, other words (with same symbol) are unaffected. Each word is an instance of a symbol (internal `symbol!` type), and exists independently of all others. `Bind` and `in` return their arguments bound. 

```
c: context [a: 2]
new: bind 'a (context? in c 'a)
; 'new now refers to 'a in context c
print get new      ; == 2
print get in c 'a  ; == 2
```

Also, you can use `:c` instead of `context? in c 'a`. They are strictly equivalent.

You can picture a context as a table with two columns, the left one for symbols, the right one with value slots. `Word`s can exist in any value container (blocks for example). `Word` = `symbol + a context pointer`. Symbols are context-free, words are context-bound.

> Why I can't change context pointer part to point to some different context, leaving older context entry as it was?
Say, `a: 1` and `a: 2` are entries in different contexts. When I encounter a word which refers to `a: 1`, why can't I alter it to point to `a: 2`, leaving `a: 1` as it was without any changes?

You can, you just need to pass a block instead of a single word value to bind. Words are scalar values, and are "passed by value" on the evaluation stack. If you bind a word directly, you are binding its instance on the stack, nothing else. When you pass a block, the words in the block are rebound. If you keep a reference to that block, you can then use the rebound words.

`Bind` called on a block traverses a block looking for `any-word!` values for which symbols exist in a context and binds these words to the context.

`Bind` is simple, it just processes the argument you provide on the stack. Since you can only get the return value back from the stack, if you pass a scalar value (`word!` value in this case) and do not use the returned value (rebound new word), your `bind` call will have no effect. If you want to keep it side-effect free on a `block!` argument, you can use `copy` or `bind/copy` (which avoids an extra internal copy).

```
red>> a: 1
== 1
red>> ctx: context [a: 2]
== make object! [
    a: 2
]
red>> get bind 'a ctx
== 2
red>> a
== 1
```
`bind 'a ctx` returns a "new" `a bound to ctx`, but has no other effect. The first 'a (`a: 1`) is still bound to the system context. `a` and `a` are two different words, which share the same symbol. Words are instances of symbols. A new word bound to the global context by default, no matter what you did to a previous word with the same spelling. 

# Scalars and value slots

Scalar values can fit in a value slot entirely (128-bits). Non-scalar values cannot, so they have a "value slot" part and one or several extra buffers. Those extra buffers are shared by default.

Values in Red cannot exist outside a value container (`block!`, `paren!`, `map!`, `hash!`, `context!`, ...), even what you write in the console, the input is `load`ed as a block.

