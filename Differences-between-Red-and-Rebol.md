# Table of Contents

1. [COPY object](#copy-object)
2. [FLOAT vs DECIMAL](#float-vs-decimal)
3. [FUNCTION vs FUNCT](#function-vs-funct)
4. [LOCAL CONTEXTS FOR LOOPS](#local-contexts-for-loops)
5. [BINDING TO SELF](#binding-to-self)
6. [INVALID BLOCK SELECTOR RETURNS NONE](#invalid-block-selector-returns-none)

## COPY object!

R2: `copy object!` is not supported.

R3: `copy object!` does not rebind functions to the new copy.

Red: `copy object!` rebinds functions to the new copy.

Example:

    >> o1: make object! [a: 42 m: does [a]]
    >> o2: copy o1
    >> o2/a: 99

    >> o1/m
    == 42         ;; Both R3 and Red
    
    ;; R3 does not rebind:
    >> o2/m       ;; R3
    == 42         ;; R3

    ;; Red rebinds:
    red>> o2/m    ;; Red
    == 99


## FLOAT! vs DECIMAL!

Different name for the datatype implementing the [standard IEEE-754 64-bit binary floating-point format](http://en.wikipedia.org/wiki/Double-precision_floating-point_format).

R2: `decimal!`

R3: `decimal!` <sup>[1](http://www.rebol.com/r3/docs/datatypes/decimal.html)</sup>

Red: `float!` <sup>[2](http://www.red-lang.org/2014/08/043-floating-point-support.html)</sup>


## FUNCTION vs FUNCT

_Summary_: Red's (and R3's) FUNCTION is like R2's FUNCT, they automatically collect /LOCAL words.

R2:
- FUNCTION is a 3-argument function constructor taking a SPEC block, a VARS (locals) block, and a BODY block. It is a mezzanine function.
- FUNCT is a 2-argument function constructor taking a SPEC block, and a BODY block. SET-WORD!s in BODY are automatically (and deeply) collected as /LOCALs of the function. It is a mezzanine function.

R3:
- FUNCTION is 2-argument auto-localising. It is a mezzanine function.
- FUNCT is an alias for FUNCTION.

Red:
- FUNCTION is 2-argument auto-localising. It is a native function.
- FUNCT does not exist.

## LOCAL CONTEXTS FOR LOOPS

R2: Loops with words for counters or iterated values have a local context for them.

R3: Loops with words counters or iterated values have a local context for them.

Red: Does not provide a local context for loops, as it has an associated high runtime cost. It requires re-BINDing (and eventually COPYing the whole body block) each time the loop is about to be evaluated.

## BINDING TO SELF

R2: You can use either ```'self``` or ```self``` when binding a word or block to the context of an object.

R3: You can use either ```'self``` or ```self``` when binding a word or block to the context of an object.

Red: R2: You can use only ```self``` when binding a word or block to the context of an object.


## INVALID BLOCK SELECTOR RETURNS NONE

Under Rebol, using an invalid index selector in a path throws an error, under Red it returns `none`. This is normally very helpful, but be careful if you're using words mapped to indexes and forget to make them a `get-word!` in the path. Used in calculations, e.g. following a `-` op, it can lead to error messages that aren't obvious:

```
*** Script error: none! type is not allowed here
*** Where: -
```

R2/R3:
```
>> blk: [image img 100x100 300x300]
== [image img 100x100 300x300]
>> IDX_TL: 3
== 3
>> blk/IDX_TL
** Script Error: Invalid path value: IDX_TL
** Near: blk/IDX_TL
>> blk/:IDX_TL
== 100x100
```

Red:
```
red>> blk: [image img 100x100 300x300]
== [image img 100x100 300x300]
red>> IDX_TL: 3
== 3
red>> blk/IDX_TL
== none
red>> blk/:IDX_TL
== 100x100
```

