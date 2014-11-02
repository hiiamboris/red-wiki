## COPY object!

R2: `copy object!` is not supported.

R3: `copy object!` does not rebind functions to the new copy.

Red: `copy object!` rebinds functions to the new copy.

Example:

    >> o1: make object! [a: 42 m: does [a]]
    >> o2: copy o1
    >> o2/a: 99

    >> o1/m
    == 42                   ;; Both R3 and Red
    
    ;; R3 does not rebind:
    >> o2/m       ;; R3
    == 42         ;; R3

    ;; Red rebinds:
    red>> o2/m    ;; Red
    == 99