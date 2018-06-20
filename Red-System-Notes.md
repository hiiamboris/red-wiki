# What do `#system` and `#system-global` do, when used in Red?

`#system` includes the argument block of Red/System code at current position in the generated R/S source code resulting from Red code compilation. `#system-global` works similarly, but places the argument code in the global space, outside of the Red runtime and user code contexts. Normally, you use routines to interface Red and R/S code, though those directives can come 
[handy](https://github.com/red/red/blob/master/environment/system.red#L359) in [some](https://github.com/red/red/blob/master/libRed/libRed.red#L16) special [cases](https://github.com/red/red/blob/master/environment/functions.red#L914).

# Literal Array Pointers

See: https://static.red-lang.org/red-system-specs.html#section-4.8.6

Literal array pointers are always defined as int-ptr! regardless of the content of the array (otherwise it wouldn't be able to access the size nor support mixed type arrays). So if you want to access, e.g., floats, you need to acquire a float pointer first:

```
powers: [1.0 2.0 3.0]
floats: as float-ptr! powers
probe floats/1
```