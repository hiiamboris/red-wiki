****Through the follow sections, the aim is to list differences between Red and Rebol and to provide a way for the Rebol Guide examples to run correctly, if possible, when there is an issue in Rebol/Red compatibility.****


__****Not all examples are run-able in the Rebol guide, even by Rebol. They are examples as in, 'For example', or 
just to illustrate a point.****__


## Rebol/Core    Chapter 3 - Quick Tour

http://www.rebol.com/docs/core23/rebolcore-3.html

### 2.2 Times

"Seconds can include a decimal sub-second. Times can also include AM and PM appended without intervening spaces:
12:35PM 9:15AM"

AM and PM do not work in Red.

### 2.3 Dates

[Red date! documentation](https://doc.red-lang.org/en/date.html)

### 2.4
No money! datatype in Red.

### 2.5 Tuples

`2.3.0   ==  tuple!`

2.3. 
```
*** Syntax Error: invalid float! at "2.3."
*** Where: do
*** Stack: load 
```

Decimal is Float in Red:
```
>> type? 2.3
== float!
```

### 2.13 Binary

"Binary values are byte strings of any length. They can be encoded directly as hexadecimal or base-64. For example:
    #{42652061205245424F4C}
    64#{UkVCT0wgUm9ja3Mh}
"
(check Red rules)
  
`help enbase`

`help debase`

### 4. Blocks

The examples are not all run-able. They are examples as in, 'For example'.
`if time > 10:30 [send jim news]` will produce an error. Note that some of the examples will also produce errors in Rebol.  They are not meant to run, just to illustrate a point.

```
foreach [site action] sites [
    data: read site
    do action
]
```
Will freeze the Red GUI Console.

### 6. Evaluation

`if current-time > snack-time [print snack-time]`

Produces an error: 
```
*** Script Error: current-time has no value
*** Where: >
*** Stack:
```

```
loop 20 [
    wait 8:00
    send friend@rebol.com read http://www.cnn.com
]
```
(Do not run unless you want your terminal to hang)

`send` is not defined in Red.

### 7. Functions

Rebol function is: `FUNCTION spec vars body`

Red function is: `FUNCTION spec body`

Red will accept the form of Rebols function, but will generate an error when the function is
called.
```
average: function [series] [total] [
    total: 0
    foreach value series [total: total + value]
    total / (length? series)
]
```

`print average [37 1 42 108]`
```
*** Script Error: total has no value
*** Where: average
*** Stack: average  
```

### 9. Object

No money! type in Red. Examples have to be modified in order to run.
For example, use `balance: 100` instead of `balance: $100`

### 10. Scripts

REBOL `page-sum: checksum page` 

Red   `page-sum: checksum page 'SHA256`

Consult `help checksum`.

```
if any [
    not exists? %page-sum.r
    page-sum <> (load %page-sum.r)
][
    print ["Page Changed" now]
    save %page-sum.r page-sum
    send luke@rebol.com page
]
```

`send` is not defined in Red.

`send luke@rebol.com page` cannot be used.

### 11. Files

`print modified? %file.txt`

`modified?` is not defined in Red.

### 12. Networking

Currently, only HTTP is available in Red.


***

## REBOL/Core Chapter 4 - Expressions

http://www.rebol.com/docs/core23/rebolcore-4.html

### 4.3 Evaluating Blocks

```
if time > 12:30 [print "past noon"]
```

Should be:

```
if now/time > 12:30 [print "past noon"]
```

### 4.4 Reducing Blocks

```
print reform [1 + 2  3 + 4]
3 7
print remold [1 + 2  3 + 4]
[3 7]
```

`reform` and `remold` do not exist in Red. Use `form reduce` and `mold reduce` instead.

### 5. Words

`zero` is not defined in Red by default.

### 5.7 Protecting Words

Currently, Red has no `protect` or `unprotect` functions.


### 7.3 For

There is no `for` function in Red.

### 7.4 Foreach

```
movies: [
     8:30 "Contact"      $4.95
    10:15 "Ghostbusters" $3.25
    12:45 "Matrix"       $4.25
]

foreach [time title price] movies [
    print ["watch" title "at" time "for" price]
]
watch Contact at 8:30 for $4.95
watch Ghostbusters at 10:15 for $3.25
watch Matrix at 12:45 for $4.25
```

There is no `money!` datatype in Red. Use `float`, instead. 

```
movies: [
     8:30 "Contact"      4.95
    10:15 "Ghostbusters" 3.25
    12:45 "Matrix"       4.25
]

foreach [time title price] movies [
    print ["watch" title "at" time "for" price]
]
watch Contact at 8:30 for 4.95
watch Ghostbusters at 10:15 for 3.25
watch Matrix at 12:45 for 4.25
```

### 7.5 Forall and Forskip

"When forall returns, the color index is at the tail of the series."

The series is set back to `head` before `forall` exits.

```
>> forall colors [print first colors]
red
green
blue
>> probe colors
[red green blue]
== [red green blue]
>> 
```

There is no `forskip` in Red.

### 8.2 Switch

```
str: copy "right "

print switch 22 [
    11 [join str "here"]
    22 [join str "there"]
]
```
Since Red currently has no `join` function, equivalent code would be:

```
str: copy "right "

print switch 22 [
    11 [append str "here"]
    22 [append str "there"]
]
```

```
person: 123
switch type?/word [
    string! [print "a string"]
    binary! [print "a binary"]
    integer! [print "an integer number"]
    decimal! [print "a decimal number"]
]
```

Appears to be wrong. Should be?:

```
person: 123
switch type?/word person [
    string! [print "a string"]
    binary! [print "a binary"]
    integer! [print "an integer number"]
    decimal! [print "a decimal number"]
]
```

`send` is not defined in Red. The following example will not work.

```
time: 12:30
switch time [
     8:00 [send wendy@domain.dom "Hey, get up"]
    12:30 [send cindy@rebol.dom "Join me for lunch."]
    16:00 [send group@every.dom "Dinner anyone?"]
]
```

### 8.2.2 Common Cases

This code:
```
case1: [print length? url]   ; the common block

url: http://www.rebol.com
switch url [
    http://www.rebol.com case1
    http://www.cnet.com [print "there"]
    ftp://ftp.rebol.org case1
]
```

Should be:

```
case1: [print length? url]   ; the common block

url: http://www.rebol.com
switch url [
    http://www.rebol.com [do case1]
    http://www.cnet.com [print "there"]
    ftp://ftp.rebol.org [do case1]
]
```

### 9. Stopping Evaluation

```
if time > 12:00 [halt]
```

Should be:

```
if now/time > 12:00 [halt]
```

### 10. Trying Blocks

```
for num 5 0 -1 [
    if error? try [print 10 / num] [print "error"]
]
```

`for` does not exist in Red.



***

## REBOL/Core Chapter 6 - Series

http://www.rebol.com/docs/core23/rebolcore-6.html


### 1.1 Traversing a Series

If the series position is set to `tail`in Rebol, trying to access the first element is an error.
```
print first colors
** Script Error: Out of range or past end.
** Where: print first colors
```

In Red it returns `none`.

```
>> print first colors
none
```

### 4.1 Length?

Typo in: 
```
color: next color
print length? color
```

Should be:
```
colors: next colors
print length? colors
```

### 4.5 Offset?

Typo:
```
data: [1 2 3 4]
data1: next data
data2: back tail data
print offset? data1 data2
4
```

Result should be 2.


### 5. Making and Copying Series

`list: make list! 128`
There is no list! datatype in Red.

### 6.3 Forall Loop

```
colors: [red green blue yellow orange]
forall colors [print first colors]
```

"The forall advances the variable position through the series, so when it returns the variable is left at its tail:"
```
print tail? colors
true
```
"Therefore, the variable must be reset before it is used again:
colors: head colors"

This is not true in latest version of Rebol 2, and is not true in Red.
The position is set back to head before `forall` exits.
```
>> forall colors [print first colors]
red
green
blue
yellow
orange
>> tail? colors
== false
>> head? colors
== true
```

### 6.4 Forskip Loop

There is no `forskip` in Red.

### 7.2 Refinement Summary

Red has a few different refinements for `find` than Rebol.

Consult `help find`

### 7.3 Partial Searches

" find/part takes either a count or an ending position."

In Red `find/part` takes:
```
/part        => Limit the length of the search.
        length       [number! series!] 
```

This Rebol example will fail:
```
colors: [red green blue yellow blue orange gold]
probe find/part colors 'blue
```

### 7.6 Repeated Searches

These examples are for illustrative purposes, or "for instance".
Not run-able without modification.

### 7.8 Wilcard Searches
Wildcard searches with `/any` are not yet implemented for Red.

### 7.10 Search and Replace

```
probe replace data 4 `four
```

Back-tick in text should be single quote `'`


### 11.2 Only

```
insert/only (find blk 5) [$1 $2 $3]
probe blk
```

`$` symbols must be removed to run example. Red currently has no money! type.


***

## Chapter 7 - Block Series
http://www.rebol.com/docs/core23/rebolcore-7.html

### 3. Arrays

```
arr: [
    [1   2   3  ]
    [a   b   c  ]
    [$10 $20 $30]
]
*** Syntax Error: missing #"]" at "$10 $20 $30]]"
*** Where: do
*** Stack: load
```

There is no money! dataype in Red. Change from $10 $20 $30 to 10 20 30 to run example.

### 3.1 Creating Arrays

There is currently no `array!` type in Red. Use `block!` or `vector!`

`b: make block! 5` 			Make a `block!` with room for 5 elements.

Make `vector!`  of size `n`. Initial values are `0`.
```
>> arr: make vector! 5 
== make vector! [0 0 0 0 0]

>> arr/1
== 0
```

Here is a port of the Rebol `array` function by Gregg Irwin, which you can use for the examples in this section:

```
array: function [
    "Makes and initializes a block of of values (NONE by default)"
    size [integer! block!] "Size or block of sizes for each dimension"
    /initial "Specify an initial value for elements"
        value "For each item: called if a func, deep copied if a series"
][
    if block? size [
        if tail? more-sizes: next size [more-sizes: none]
        size: first size
        if not integer? size [
            ; throw error, integer expected
            cause-error 'script 'expect-arg reduce ['array 'size type? get/any 'size]
        ]
    ]
    result: make block! size
    case [
        block? more-sizes [
            loop size [append/only result array/initial more-sizes :value]
        ]
        series? :value [
            loop size [append/only result copy/deep value]
        ]
        any-function? :value [
            loop size [append/only result value]
        ]
        'else [
            append/dup result value size
        ]
    ]
    result
]
```

### 4. Composing Blocks

Rebol example:
```
probe compose/deep [a b [c (d e)]]
[a b [c d e]]
```

Red:
```
>> probe compose/deep [a b [c (d e)]]
*** Script Error: d has no value
*** Where: compose
*** Stack: probe  
```

There seems to be some slight differences with Reds compose.
Consult `help compose`.

For the above example to behave the same as Rebols compose/deep, use `compose/only`.

```
>> probe compose/only [a b [c (d e)]]
[a b [c (d e)]]
== [a b [c (d e)]]
```


***

## Chapter 8 - String Series
http://www.rebol.com/docs/core23/rebolcore-8.html

For a list of Red string related datatypes, type: `help any-string`

The following string functions are not predefined in Red:

`join`
`reform`
`remold`

We can use Rebol 2 source to add our own versions of these functions:

`join`

```
join: func [
    "Concatenates values."
    value "Base value"
    rest "Value or block of values"
][
    value: either series? :value [copy value] [form :value]
    repend value :rest
]
```

`reform`

```
reform: func [
    "Forms a reduced block and returns a string."
    value "Value to reduce and form"
][
    form reduce :value
]
```

`remold`

```
remold: func [
    "Molds a reduced block and returns a string."
    value "Value to reduce and mold"
][
    mold reduce :value
]
```

"This chapter will also describes these string functions:"

The following functions do not currently exist in Red:

`detab`

`entab`

`compress`

### 2.1 Join

```
print join $11 " dollars"
```

There is no `money!` dataype in Red. Change $11 to 11 for example to run.

### 2.3 Form
There is no money type in Red. Change $1.50 to 1.50 to run examples.

### 2.5 Mold

```
money: $11.11
sub-blk: [inside another block mold this is unevaluated]
probe mold [$22.22 money "-- unevaluated block:" sub-blk]
```

Red does not currently have a `money!` datatype. Remove `$` symbol to run examples.

```
money: 11.11
sub-blk: [inside another block mold this is unevaluated]
probe mold [22.22 money "-- unevaluated block:" sub-blk]
```

### 2.7.2 Detab and Entab

 `detab` and `entab` functions do not exist  in Red.

### 2.8 Uppercase and Lowercase

Typo in uppercase example.  Word uppercase has three `p`s.

```
print upppercase/part "ukiah" 1
```

Should be:

```
print uppercase/part "ukiah" 1
```

### 2.9 Checksum

"By default, the CRC checksum is computed"
"CRC	24 bit circular redundancy checksum"

```
print checksum "hello"
52719
print checksum (read http://www.rebol.com/)
356358
```

Red `checksum`  requires method argument.

```
ARGUMENTS:
     data         [binary! string! file!] 
     method       [word!] {MD5 SHA1 SHA256 SHA384 SHA512 CRC32 TCP ADLER32 hash}.
```

In Red:

```
>> print checksum "hello" 'md5
#{5D41402ABC4B2A76B9719D911017C592}
```

Checksum has different refinments in Red. Consult `help checksum`.

### 2.10 Compression and Decompression

```
print [size? str "bytes"]
306 bytes
```

In Red `size` takes `file!` as its argument.

There is currently no `compress` in Red.

__Need solutions to make examples run-able.__


### 2.11 Number Base Conversion

Will not work as written:

```
b-line: debase e-line
print type? b-line
binary
probe b-line
#{4E6F2120546865726527732061206C616E6421}
print to-string b-line
No! There's a land!
```

You can use these to enable the code to be run-able:
```
line: "No! There's a land!"
e-line: enbase line
b-line: debase e-line
```

***

## Chapter 15 - Parsing
http://www.rebol.com/docs/core23/rebolcore-15.html

There are some key differences between Rebol `parse` and Reds `parse`.

The very first examples in the Rebol Guide will not work, as there is no `none` option for splitting strings.

[See parse article for Red](http://www.red-lang.org/2013/11/041-introducing-parse.html)













