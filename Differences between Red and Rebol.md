# Table of Contents

1. [COPY object](#copy-object)
2. [FUNCTION vs FUNCT](#function-vs-funct)


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


## FUNCTION vs FUNCT

_Summary_: Red's (and R3's) FUNCTION is like R2's FUNCT, they automatically collect /LOCAL words.

R2:
- FUNCTION is a 3-argument function constructor taking an ARGS block, a VARS (locals) block, and a BODY block.
- FUNCT is a 2-argument function constructor taking a SPEC block, and a BODY block. SET-WORD!s in BODY are automatically (and deeply) collected as /LOCALs of the function.

R3:
- FUNCTION is 2-argument auto-localising.
- FUNCT is an alias for FUNCTION.

Red:
- FUNCTION is 2-argument auto-localising.
- FUNCT does not exist.