The bootstrap version of Red/System had a pre-processor added to provide some features that couldn't quickly be added to this version of the compiler. Red/System was not designed to have a pre-processor and it needs to be removed before Red/System 2.0 is released.

This document suggests some alternatives that could be implemented in the compiler.

## #define a constant
 
### alternative 1
```rebol
MY_CONSTANT: [constant] "This cannot be changed"
```

This alternative follows the pattern of including some compiler directives in square brackets.

### alternative 2
```rebol
MY_CONSTANT: "This cannot be changed"
protect [MY_CONSTANT]
```

This alternative follows the Red syntax for creating a constant. In Red, a constant can be "released" by calling the 'unprotect function.

Whilst this best mirrors Red syntax, it is possible to write confusing code by separating the assignment of the value from the protect.

### alternative 3
```rebol
protect [MY_CONSTANT "This cannot be changed"]
```

A hybrid approach that could also be added to Red by making protect a variadic function.

### alternative 4
```rebol
MY_CONSTANT: declare constant! "This cannot be changed"
```

An approach based on way to declare a struct!.

## Macro (no arguments):

```rebol
my-macro: does [[inline]] [ ... macro]
```

The compiler would then inline the code inside the block. Inlining probably wouldn't be necessary if does was changed to just use jump instructions without the function overhead.

## Macro (arguments):

```rebol
my-macro-with-args: func [ [inline] arg1 [type1!] arg2 [type2!]] [... macro code]
```

This would require inlining.

