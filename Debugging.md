# Red console / interpreter

system/state/trace: on

system/preprocessor/debug?: on

## Parse

`parse/trace` allows you to debug `parse` expressions.
An example of its use is provided by the built `parse-trace`.
Simply substitute `parse` with `parse-trace` in your code.

## View / VID / React

dump-face

dump-reactions

system/view/debug?: on

system/reactivity/debug?: on

# Red/System

Use the commandline switch `-d` to get a debug build.

See an example of [debugging the red console itself](https://github.com/red/red/wiki/How-to-Debug-__-A-use-case-by-DocKimbel)

**tl;dr** :
Use 
```red
probe 
?? 
dump4 
stack/dump 
print-symbol
```

Remember that Red's toolchain is minimal, building from source is fast and easy, and the source base is small and well organized. And if you've only ever worked at the Red level, a small amount of time learning some Red/System can go a long way. Even if you aren't confident you could write a fix, you may help save others time, and have fun exploring in the process. Don't be afraid.

# See also:

- https://github.com/red/red/ (readme shows command line switches, of which a few are debugging related)
