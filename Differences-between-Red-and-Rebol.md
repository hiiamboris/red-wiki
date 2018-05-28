# Table of Contents

1. [COPY object](#copy-object)
2. [FLOAT vs DECIMAL](#float-vs-decimal)
3. [FUNCTION vs FUNCT](#function-vs-funct)
4. [LOCAL CONTEXTS FOR LOOPS](#local-contexts-for-loops)
5. [BINDING TO SELF](#binding-to-self)
6. [INVALID BLOCK SELECTOR RETURNS NONE](#invalid-block-selector-returns-none)
7. [OBJECT INTROSPECTION](#object-introspection)
8. [DIR? FUNCTION](#dir-function)
9. [TO-DATE FUNCTION](#to-date-function)
10. [RANDOM](#random)
11. [DO](#do)
12. [MAKE NONE](#make-none)

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

Red: You can use only ```self``` when binding a word or block to the context of an object.


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

## OBJECT INTROSPECTION

In Rebol2, an object! was inspected using the ```first```, ```second``` and ```third``` functions. ```first``` provided a block containing the words of the object including ```self``` as the first entry. ```second``` provided a block containing the values of the object again including the value of ```self``` as the beginning of the block. (The value of ```self``` being ```make object! [<object body>]```.) ```third``` returns block containing the object body and does not include ```self```. The body of an object is its complete spec block of its current state, which can be used to make the object.

The ```first```, ```second``` and ```third``` functions do not work on object! values in Rebol3 or Red. They were replaced by the ```words-of```, ```values-of``` and ```body-of``` functions that return a block of words, a block of values and the object's body respectively. They do not include ```self```.

Later versions of Rebol2 include ```words-of```, ```values-of``` and ```body-of``` though retain object introspection through ```first```, ```second``` and ```third```.  

## DIR? FUNCTION

Under R2, `dir?` returns true based on whether the target is an actual directory on disk. Under Red, it returns true if the target *looks like* a directory, because the name ends with a path separator.

## TO-DATE FUNCTION

In Rebol2 and Rebol3 ```to-date``` function works with string values but not in Red yet. So currently `to-date "2-May-2018"` will return an error. ```load``` can be used instead.

Please note that this is a temporary difference since Red's lexer in not in its final form yet.

## RANDOM

In Rebol2, `random` copies the series argument before shuffles, in Rebol3 and Red, it modifies:

```
>> s: "12345"
== "12345"
>> random s
== "43512"
>> s
== "43512" ("12345" in R2)
```

## DO

The values that get special treatment by do are: [block! path! string! url! file! error!] Everything else is evaluated passively. This is by design, to eliminate variable arity.

On R2 and R3
```
>> type? do func [][1]
== integer!
```

on Red
```
>> type? do func [][1]
== function!
```

## MAKE NONE

`make` generally doesn't accept `none` in spec argument on Red and raise error, which is compatible with Rebol3 but is different from Rebol2:

```
make integer! none ; == 0
make block!   none ; == []
make file!    none ; == %""
make tag!     none ; == <>
...
```

Above lines give error on Red. Similar issue happens with "" (empty string)

```
make integer! "" ; == 0 (R2)
make path!    "" ; <empty path>
...
```

These works on Rebol2 but not on Red / Rebol3.

Also notice below difference:

```
m: make hash! none
length? m
```

This returns 0 on Rebol2 but 1 on Red.
Red creates a hash with one value (none) inside, but Rebol2 creates an empty hash.
