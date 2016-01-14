# Table of Contents

1. [COPY object](#copy-object)
2. [FLOAT vs DECIMAL](#float-vs-decimal)
3. [FUNCTION vs FUNCT](#function-vs-funct)
4. [LOCAL CONTEXTS FOR LOOPS](#local-contexts-for-loops)


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