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

