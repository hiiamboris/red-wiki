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
13. [LAST](#last)
14. [TAKE](#take)
15. [TO-INTEGER](#to-integer)
16. [LOAD](#load)
17. [READ](#read)
18. [ADJUST TIME BY SETTING TIMEZONE](#adjust-time-by-setting-timezone)
19. [REPEND](#repend)
20. [PARSE](#parse)
21. [TO-TIME](#to-time)
22. [SELECT SKIP](#select-skip)

## COPY object!

R2: `copy object!` is not supported.

R3: `copy object!` does not rebind functions to the new copy.

Red: `copy object!` rebinds functions to the new copy.

Example:

```red
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
```

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

```red
*** Script error: none! type is not allowed here
*** Where: -
```

R2/R3:

```rebol
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

```red
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

```
>> load "2018/01/02"
== 2-Jan-2018

>> to-date "2018/01/02"
Script Error: cannot MAKE/TO date! from: "2018/01/02"
```

Please note that this is a temporary difference since Red's lexer in not in its final form yet.

## RANDOM

In Rebol2, `random` copies the series argument before shuffles, in Rebol3 and Red, it modifies:

```red
>> s: "12345"
== "12345"
>> random s
== "43512"
>> s
== "43512" ("12345" in R2)
```

## DO

The values that get special treatment by do are: [block! path! string! url! file! error!] Everything else is evaluated passively. This is by design, to eliminate variable arity. (See [here](https://gitter.im/red/help?at=5ab285275f188ccc15df954b))

On R2 and R3
```rebol
>> type? do func [][1]
== integer!
```

on Red
```red
>> type? do func [][1]
== function!
```

## MAKE NONE

`make` generally doesn't accept `none` in spec argument on Red and raise error, which is compatible with Rebol3 but is different from Rebol2:

```red
make integer! none ; == 0
make block!   none ; == []
make file!    none ; == %""
make tag!     none ; == <>
...
```

Above lines give error on Red. Similar issue happens with "" (empty string)

```red
make integer! "" ; == 0 (R2)
make path!    "" ; <empty path>
...
```

These works on Rebol2 but not on Red / Rebol3.

Also notice below difference:

```red
m: make hash! none
length? m
```

This returns 0 on Rebol2 but 1 on Red.
Red creates a hash with one value (none) inside, but Rebol2 creates an empty hash.

## LAST

`last []` returns `none` on Red and R3, but returns `Out of range or past end` on R2.

## TAKE

`take []` returns `none` on all Red, R3 and R2.

But `take/part [] 1` returns empty block on R2 and R3, but `none` on Red. Red's behavior looks more consistent.

## TO-INTEGER

`to-integer ""` returns error on Red and R3, but `0` on R2.

## LOAD

1. `load %file.txt` loads the content of the file and return one value (if there is just one value in the file) or a block of values (if there are more values in the file) on Red and R2, but returns a string on R3.

```
>> load %/c/file.txt
== [abc def] ; Red & R2
== "abc^/def^/" ; R3
```

2. `load %./` returns a block of files and folders on R2 and R3, but not on Red:

```
>> load %/c/
== [%$Recycle.Bin/ %$WINDOWS.~BT/ %Boot/ %bootmgr ... ] ; R2 & R3
*** Script Error: transcode does not allow block! for its <anon> argument ; Red
```

## READ

`read/lines/part` behaves different on Red, R2 and R3:

```
>> write/lines %file.txt ["one" "two" "three"]

>> read/lines/part %file.txt 2
== ["on"] ; Red & R3
== ["one" "two"] ; R2

>> read/lines/part %file.txt 4
== ["one^M"] ; Red
== ["one"] ; R3
== ["one" "two" "three"] ; R2
```

Note that full IO support will come with v0.7.0 to Red and current simple-io functionality may change.

## ADJUST TIME BY SETTING TIMEZONE

When setting timezone of a date value by `zone`, time will not be adjusted in R2, R3 and Red.

When `timezone` is used to set, time adjusted, only Red has this feature:

```
>> d: now
== 22-Dec-2018/1:53:52+03:00
>> d/timezone: 5:0:0
== 5:00:00
>> d
== 22-Dec-2018/3:53:52+05:00
```

## REPEND

`repend` in Rebol is just a shortcut for the common `append ... reduce` pattern. This means that you still incur the cost of an extra block produced by reduce before append.

In Red, `repend` is the fusion of the `append ... reduce` pattern, so that no intermediary block is created (thanks to the efficient `reduce/into` call). This means that values are reduced and appended one by one in this case. It will behave the same way as `append ... reduce`, unless you rely on side-effects, like above modifying the accumulating series while reducing. In such cases, just fall back on `append ... reduce` in order to separate fully the reduction from the appending actions.

See the details [here](https://github.com/red/red/issues/3340)

## PARSE

`path!`s are evaluated in R2 and R3 but not in Red:

```
>> b: [x 3]
>> parse "aaa" [b/x "a"]
== true (on R2 & R3)
== false (on Red)
```

There is already a ticket about it and it may change in the future: [#3528](https://github.com/red/red/issues/3528)

Also check TO / THRU difference described in this issue [#3679](https://github.com/red/red/issues/3679)

## TO-TIME

In Red Meridian designations are not part of the time! form:
```
>> load "01:00PM"
== [1:00:00 PM]        ;Red
== 13:00               ;R2 & R3

>> load "13:00PM"
== [13:00:00 PM]       ;Red
== Invalid time error  ;R2 & R3
```

See [specs](https://doc.red-lang.org/en/datatypes/time.html#_literal_syntax)

This leads to below differences:

```
>> to time! "1:00PM"
== 01:00:00 ;Red
== 13:00    ; R2
== 13:00    ;R3
```

```
>> to time! "13:00PM"
== 13:00 ;Red
== none  ;R2
== Error ;R3
```

## SELECT SKIP

`select/skip` returns a block (the record, see the help `? select`) in R2, returns a single value in R3 & Red:

```
>> select/skip [1 2 3 4 5 6] 1 3
== [2 3] ;R2
== 2     ;R3 & Red
```
