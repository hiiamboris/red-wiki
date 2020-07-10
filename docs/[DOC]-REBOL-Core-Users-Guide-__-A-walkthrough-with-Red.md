**Chapters:**
<a name="top">[3](#chap3)  [4](#chap4)  [5](#chap5)  [6](#chap6)  [7](#chap7)  [8](#chap8)  [10](#chap10)  [12](#chap12) [15](#chap15)

****This walk-through explores differences between Red and Rebol and attempts to provide ways to run the code examples in the Rebol/Core Guide where incompatibilities or typos arise.****


Note:
Not all examples are run-able in the Rebol guide, even by Rebol. Some examples are, 'For example', or 
just to illustrate a point.



## <a name="chap3">Rebol/Core    Chapter 3 - Quick Tour

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-3.html


### 2.2 Times

`Seconds can include a decimal sub-second. Times can also include AM and PM appended without intervening spaces:
12:35PM 9:15AM`

AM and PM do not work in Red.


### 2.3 Dates

[Red date! documentation](https://doc.red-lang.org/en/datatypes/date.html)

### 2.5 Tuples

`2.3.0   ==  tuple!`

A trailing `.` is an error in Red:

2.3. 

```red
*** Syntax Error: invalid float! at "2.3."
*** Where: do
*** Stack: load 
```

Decimal in Rebol is `float!` in Red:
```red
>> type? 2.3
== float!
```

### 2.13 Binary

```red
Binary values are byte strings of any length. They can be encoded directly as hexadecimal or base-64. 

For example:

#{42652061205245424F4C}

64#{UkVCT0wgUm9ja3Mh}
```

Consult `help` in the Red cosole for `enbase` and `debase` specifics.

`help enbase`

`help debase`


### 6. Evaluation

> if current-time > snack-time [print snack-time]

This code is just an example and will produce an error if you attempt to run it: 
```red
*** Script Error: current-time has no value
*** Where: >
*** Stack:
```

```red
loop 20 [
    wait 8:00
    send friend@rebol.com read http://www.cnn.com
]
```

`send` is not defined in Red.


### 7. Functions

Rebol function is: `FUNCTION spec vars body`

Red function is: `FUNCTION spec body`

Red will accept the form of Rebols function, but will generate an error when the function is
called.
```red
average: function [series] [total] [
    total: 0
    foreach value series [total: total + value]
    total / (length? series)
]
```

`print average [37 1 42 108]`
```red
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

```red
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

`modified?` is not defined in Red. You can use `query` on files for now, but it's a subject to redesign, and returns a UTC timestamp.


### 12. Networking

Currently, only HTTP is available in Red.


***

## <a name="chap4"> REBOL/Core Chapter 4 - Expressions

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-4.html


### 4.3 Evaluating Blocks


> if time > 12:30 [print "past noon"]

Should be:

```red
if now/time > 12:30 [print "past noon"]
```


### 4.4 Reducing Blocks

```red
print reform [1 + 2  3 + 4]
3 7
print remold [1 + 2  3 + 4]
[3 7]
```

`reform` and `remold` do not exist in Red. Use `form reduce` and `mold reduce` instead.

You can create your own functions if you want `reform` and `remold`.

`reform: func [val][form reduce val]`

`remold: func [val][mold reduce val]`



### 5. Words

`zero` is not defined in Red by default.


### 5.7 Protecting Words

Currently, Red has no `protect` or `unprotect` functions.


### 7.3 For

There is no `for` function in Red.


### 7.4 Foreach

```red
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

```red
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

> When forall returns, the color index is at the tail of the series.

The series is set back to `head` before `forall` exits.

```red
>> forall colors [print first colors]
red
green
blue
>> probe colors
[red green blue]
== [red green blue]
>> 
```

```red
>> head? colors
== true
```

There is no `forskip` in Red.


### 8.2 Switch

```red
str: copy "right "

print switch 22 [
    11 [join str "here"]
    22 [join str "there"]
]
```
Since Red currently has no `join` function, equivalent code would be:

```red
str: copy "right "

print switch 22 [
    11 [append str "here"]
    22 [append str "there"]
]
```

This example seems to missing the `person` word in the switch:
```red
person: 123
switch type?/word [
    string! [print "a string"]
    binary! [print "a binary"]
    integer! [print "an integer number"]
    decimal! [print "a decimal number"]
]
```

Should be?:

```red
person: 123
switch type?/word person [
    string! [print "a string"]
    binary! [print "a binary"]
    integer! [print "an integer number"]
    decimal! [print "a decimal number"]
]
```

`send` is not defined in Red. The following example will not work.

```red
time: 12:30
switch time [
     8:00 [send wendy@domain.dom "Hey, get up"]
    12:30 [send cindy@rebol.dom "Join me for lunch."]
    16:00 [send group@every.dom "Dinner anyone?"]
]
```


### 8.2.2 Common Cases

This code:
```red
case1: [print length? url]   ; the common block

url: http://www.rebol.com
switch url [
    http://www.rebol.com case1
    http://www.cnet.com [print "there"]
    ftp://ftp.rebol.org case1
]
```

Should be:

```red
case1: [print length? url]   ; the common block

url: http://www.rebol.com
switch url [
    http://www.rebol.com [do case1]
    http://www.cnet.com [print "there"]
    ftp://ftp.rebol.org [do case1]
]
```


### 9. Stopping Evaluation

```red
if time > 12:00 [halt]
```

Should be:

```red
if now/time > 12:00 [halt]
```

### 10. Trying Blocks

```red
for num 5 0 -1 [
    if error? try [print 10 / num] [print "error"]
]
```

`for` does not exist in Red.


***

## <a name="chap5">Chapter 5 - Scripts

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-5.html


### 2. Prefaced Scripts

> Text that appears before the header is called the preface and is ignored during evaluation.

```red
The text that appears before the header is ignored
by REBOL and can be used for comments, email headers,
HTML tags, etc.

REBOL [
  Title:   "Preface Example"
  Date:    8-Jul-1999
]

print "This file has a preface before the header"
```

"," is one of the invalid `word` characters in Rebol and Red, so an Error will occur if  `load` is used:
```red
*** Syntax Error: invalid value at ", email headers,HTML tags, etc.Red ["
*** Where: do
*** Stack: load 
```

Remove commas from the Preface and change the header to `Red []` if you want to load it.


### Embedded Scripts

```red
Here is some text before the script.
[
    Red [
        Title:   "Embedded Example"
        Date:    8-Nov-1777
    ]
    print "done"
]
Here is some text after the script.
```

Error in Red whe `do` is used:

```red
*** Syntax Error: invalid value at "]Here is some text after the script."
*** Where: do
*** Stack: do-file expand-directives load  
```

Embedded scripts are not supported in Red yet.

### 3. Script Arguments

The `system/script` object for Red contains the following fields that have initial values of `none`:

`title`

`header`

`parent`

`path`

`args`

### 3.1 Program Options

To better see the `system/options` object, use `help`.

`boot`, `path`, and `cache` reflect my current setup in the example below:

```red
>> help system/options
     boot             string!       "C:\ProgramData\Red\gui-console-2018-4-1-19387.exe"
     home             none!         none
     path             file!         %/C/Users/Old%20Man/
     script           none!         none
     cache            file!         %/C/Users/Old%20Man/AppData/Roaming/Red/
     thru-cache       none!         none
     args             none!         none
     do-arg           none!         none
     debug            none!         none
     secure           none!         none
     quiet            logic!        false
     binary-base      integer!      16
     decimal-digits   integer!      15
     module-paths     block!        length: 0  []
     file-types       none!         none
     float            object!       [pretty? full? on-change*]
     on-change*       function!     [word old new]
     on-deep-change*  function!     [owner word target action new index part]
```

### 4. Running Scripts

`red script.red` Or `red --cli script.red`

### 4.1 Loading Scripts

Red script extensions are `.red` by convention.
`load` in Red accepts the following types:

`file!`, `url!`, `string!`, `binary!`

`load/header` is not yet implemented for Red.

`load/markup` is not available in Red at this time.

Consult `help load`


### 4.2 Saving Scripts

Red has no `money!` datatype. Remove the `$` from the example.
```red
>> data: [Buy 100 shares at 20.00 per share]
== [Buy 100 shares at 20.0 per share]
>> save %data.red data
```

Loaded later with:
```red
>> data: load %data.red
== [Buy 100 shares at 20.0 per share]
```

```red
>> save %date.red now
>> stamp: load %date.red
== 27-Oct-2017/22:57:10-06:00
```

```red
>> save/header %data.red data header
>> load %data.red
== [Red [
    Title: "This is an example"
] 
    Buy 100 shares at 20.0 per share
]
```

### 5.1.1 Indent Content for Clarity

The examples in this section are only for illustrative purposes.

### 5.1.3

`detab` and `entab` are not available in Red.

Gist with test cases for entab/detab mezzanine functions can be found here: [Entab/Detab](https://gist.github.com/greggirwin/c56da21b5d2b5536424acff1fdc258a5)


***

## <a name="chap6">REBOL/Core Chapter 6 - Series

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-6.html


### 1.1 Traversing a Series

> If the series position is set to tail in Rebol, trying to access the first element is an error.

```red
print first colors
** Script Error: Out of range or past end.
** Where: print first colors
```

In Red it returns `none`.

```red
>> print first colors
none
```

### 4.1 Length?

Typo in: 
> color: next color
> print length? color


Should be:
```red
colors: next colors
print length? colors
```


### 4.5 Offset?

Typo:
```red
 data: [1 2 3 4]
 data1: next data
 data2: back tail data
 print offset? data1 data2
 4
```

Result should be 2.


### 5. Making and Copying Series

> list: make list! 128

There is no list! datatype in Red.


### 6.3 Forall Loop

```red
colors: [red green blue yellow orange]
forall colors [print first colors]
```

> The forall advances the variable position through the series, so when it returns the variable is left at its tail:

```red
print tail? colors
true
```

> Therefore, the variable must be reset before it is used again:
`colors: head colors`

This is not true in latest version of Rebol 2, and is not true in Red.
The position is set back to head before `forall` exits.

```red
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

There are some differences in the refinements for `find` in Red.

Consult `help find` for details.


### 7.3 Partial Searches

> find/part takes either a count or an ending position.

In Red `find/part` takes:
```red
/part        => Limit the length of the search.
        length       [number! series!] 
```

This Rebol example will fail:
```red
colors: [red green blue yellow blue orange gold]
probe find/part colors 'blue
```


### 7.6 Repeated Searches

These examples are for illustrative purposes, or "for instance".
Not run-able without modification.


### 7.8 Wildcard Searches
Wildcard searches with `/any` are not yet implemented for Red.


### 7.10 Search and Replace

> probe replace data 4 `four


Back-tick in text should be single quote `'`


### 11.2 Only
```red
 insert/only (find blk 5) [$1 $2 $3]
 probe blk
```

`$` symbols must be removed to run example. Red currently has no money! type.


***

## <a name="chap7">Chapter 7 - Block Series

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-7.html


### 3. Arrays

```red
arr: [
    [1   2   3  ]
    [a   b   c  ]
    [$10 $20 $30]
]
*** Syntax Error: missing #"]" at "$10 $20 $30]]"
*** Where: do
*** Stack: load
```

There is no money! datatype in Red. Change from $10 $20 $30 to 10 20 30 to run example.


### 3.1 Creating Arrays

There is currently no `array!` type in Red. Use `block!` or `vector!`

`b: make block! 5` Make a block with room for 5 elements.

Make `vector!`  of size `n`. Initial values are `0`.
```red
>> arr: make vector! 5 
== make vector! [0 0 0 0 0]

>> arr/1
== 0
```

Here is a port of the Rebol `array` function by Gregg Irwin, which you can use for the examples in this section:

```red
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
```red
probe compose/deep [a b [c (d e)]]
[a b [c d e]]
```

Red:
```red
>> probe compose/deep [a b [c (d e)]]
*** Script Error: d has no value
*** Where: compose
*** Stack: probe  
```

There seems to be some slight differences with Reds compose.
Consult `help compose`.

For the above example to behave the same as Rebols compose/deep, use `compose/only`.

```red
>> probe compose/only [a b [c (d e)]]
[a b [c (d e)]]
== [a b [c (d e)]]
```


***

## <a name="chap8">Chapter 8 - String Series

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-8.html

For a list of Red string related datatypes, type: `help any-string`

The following string functions are not predefined in Red:

`join`
`reform`
`remold`

We can use Rebol 2 source to add our own versions of these functions:

`join`

```red
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

```red
reform: func [
    "Forms a reduced block and returns a string."
    value "Value to reduce and form"
][
    form reduce :value
]
```

`remold`

```red
remold: func [
    "Molds a reduced block and returns a string."
    value "Value to reduce and mold"
][
    mold reduce :value
]
```

> This chapter will also describes these string functions:

The following functions do not currently exist in Red:

`detab`

`entab`

`compress`

Gist with test cases for entab/detab mezzanine functions can be found here: [Entab/Detab](https://gist.github.com/greggirwin/c56da21b5d2b5536424acff1fdc258a5)


### 2.1 Join

```red
print join $11 " dollars"
```

There is no `money!` datatype in Red. Change $11 to 11 for example to run.


### 2.3 Form
There is no money type in Red. Change $1.50 to 1.50 to run examples.


### 2.5 Mold

```red
money: $11.11
sub-blk: [inside another block mold this is unevaluated]
probe mold [$22.22 money "-- unevaluated block:" sub-blk]
```

Red does not currently have a `money!` datatype. Remove `$` symbol to run examples.

```red
money: 11.11
sub-blk: [inside another block mold this is unevaluated]
probe mold [22.22 money "-- unevaluated block:" sub-blk]
```

### 2.7.2 Detab and Entab

 `detab` and `entab` functions do not exist in Red.

Gregg Irwin and Steeve have provided entab/detab mezzanines to use if you wish:


```red
; Steeve gets credit for this one
detab: function [
    "Converts leading tabs in a string to spaces. (tab size 4)"
	string [any-string!] "(modified)"
	/size
	    sz [integer!] "Number of spaces per tab"
	/all "Change all, not just leading"
][
	sz: max 1 any [sz 4]						; size must be at least 1
	buf: append/dup clear "    " space sz
	parse string [
		 BOL: some [
			; If we see a tab, mark its position, then change it to the...ready?
			; offset into our buffer of spaces, based on the the difference
			; between our current position and the beginning of the line, modulo
			; the tab size.
			pos: change tab (skip buf (offset? BOL pos) % sz)
			| lf BOL:							; Set our beginning-of-line marker
			| if (all) to [tab | lf]			; Skip over non-tab chars
			| space								; Spaces just move us forward in the line
			| thru lf BOL:						; Skip to end of line and set our marker
		 ]
	]
	string
]
```

```red
entab: function [
	"Converts leading spaces in a string to tabs. (tab size 4)"
	string [any-string!] "(modified)"
	/size
	    sz [integer!] "Number of spaces per tab (must be at least 1)"
	/all "Change all, not just leading"
][
	sz: max 1 any [sz 4]						; size must be at least 1
	sz-1: max 1 sz - 1							; can't have 0 as a m..n size for parse
	parse string [
		some [
			any [change sz space tab | change [1 sz-1 space tab] tab]
			if (not all) [thru [newline | end]]
			| skip
		]
	]
	string
]
```

Gist with test cases for entab/detab mezzanine functions can be found here: [Entab/Detab](https://gist.github.com/greggirwin/c56da21b5d2b5536424acff1fdc258a5)



### 2.8 Uppercase and Lowercase

Typo in uppercase example.  Word uppercase has three `p`s.

> print upppercase/part "ukiah" 1


Should be:

```red
print uppercase/part "ukiah" 1
```

### 2.9 Checksum

```red
 By default, the CRC checksum is computed
 CRC	24 bit circular redundancy checksum
 print checksum "hello"
 52719
 print checksum (read http://www.rebol.com/)
 356358
```

Red `checksum`  requires method argument.

```red
ARGUMENTS:
     data         [binary! string! file!] 
     method       [word!] {MD5 SHA1 SHA256 SHA384 SHA512 CRC32 TCP ADLER32 hash}.
```

In Red:

```red
>> print checksum "hello" 'md5
#{5D41402ABC4B2A76B9719D911017C592}
```

Checksum has different refinements in Red. Consult `help checksum`.


### 2.10 Compression and Decompression

```red
print [size? str "bytes"]
306 bytes
```

In Red `size` takes `file!` as its argument.

There is currently no `compress` in Red.


### 2.11 Number Base Conversion

Will not work as written:

```red
b-line: debase e-line
print type? b-line
binary
probe b-line
#{4E6F2120546865726527732061206C616E6421}
print to-string b-line
No! There's a land!
```

You can use these lines to make the examples run-able:
```red
line: "No! There's a land!"
e-line: enbase line
b-line: debase e-line
```

***

## <a name="chap10">Chapter 10 - Objects

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-10.html

Red has a shortcut for make object!. `example: object [x: 1 y: 2]`

There is no `money!` datatype in Red. 
Remove `$` symbols and use `float!` or `number!` datatypes in function examples to make them run.

For example:

REBOL

```red
last-account: 89431
bank-bonus: $10.00

make-account: func [
    "Returns a new account object"
    f-name [string!] "First name"
    l-name [string!] "Last name"
    start-balance [money!] "Starting balance"
][
    last-account: last-account + 1
    make bank-account [
        first-name: f-name
        last-name: l-name
        account: last-account
        balance: start-balance + bank-bonus
    ]
]
```

Red

```red
last-account: 89431
bank-bonus: 10.00

make-account: func [
    "Returns a new account object"
    f-name [string!] "First name"
    l-name [string!] "Last name"
    start-balance [number!] "Starting balance"
][
    last-account: last-account + 1
    make bank-account [
        first-name: f-name
        last-name: l-name
        account: last-account
        balance: start-balance + bank-bonus
    ]
]
```


### 6. Prototype Objects

Once an object  has been created, it can be used as a prototype for other objects. 

Create a prototype object:  `my-prototype: object [x: 3 y: 2]`

Clone the prototype object: `example1: make my-prototype []`

You can extend the cloned object by adding new words in the block:
```red
>> example2: make my-prototype [z: 1]
== make object! [
    x: 3
    y: 2
    z: 1
]
```


### 8. Encapsulation

Typo in example:
```red
Bank: make object! [

    last-account: 89431
    bank-bonus: $10.00

    set 'make-account func [
        "Returns a new account object"
        f-name [string!] "First name"
        l-name [string!] "Last name"
        start-balance [money!] "Starting balance"
    ][
        last-account: last-account + 1
        make bank-account [
            first-name: f-name
            last-name: l-name
            account: last-account
            balance: start-balance + bank-bonus
        ]
    ]
]
```

Should be:

```red
bank-account: make object! [

    last-account: 89431
    bank-bonus: 10.00

    set 'make-account func [
        "Returns a new account object"
        f-name [string!] "First name"
        l-name [string!] "Last name"
        start-balance [number!] "Starting balance"
    ][
        last-account: last-account + 1
        make bank-account [
            first-name: f-name
            last-name: l-name
            account: last-account
            balance: start-balance + bank-bonus
        ]
    ]
]
```

`bob: make-account "Bob" "Baker" 4000`


### 9. Reflective Properties

Red uses *-of functions for object reflection instead of `first` and `next first`.

```red
>> probe first luke
*** Script Error: first does not allow object! for its s argument
*** Where: first
*** Stack: probe first  
```

`body-of`
```red
>> body-of luke
== [last-account: 89433 bank-bonus: 10.0 first-name: "Luke" last-name: "Lakeswimmer" account: 89431 balance: 1204.52]
```

`class-of`

```red
>> class-of luke
== 1000006
```

`keys-of`

```red
>> keys-of luke
== [last-account bank-bonus first-name last-name account balance]
```

`values-of`

```red
>> values-of luke
== [89433 10.0 "Luke" "Lakeswimmer" 89431 1204.52]
```

`words-of`

```red
>> words-of luke
== [last-account bank-bonus first-name last-name account balance]
```

***

## Chapter 11 - Math

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-11.html

Note: There is no money data type in Red. 
Remove `$` characters where necessary to run examples.


### 2. Scalar Data Types

Red has the following numerical data types:
`integer!`

`float!`

`time!`

`print - 2:20` will produce an error. 
There must be no space between `-` and the number in Red.
Should be: `print -2:20`

`date!`


`pair!`

The `//` operator does not work with `pair!` in Red.

```red
>> print 101x32 // 10x3
*** Script Error: cannot compare 1x2 with 0
*** Where: <
*** Stack: mod 
```

```red
>> print 101x32 // 10
*** Script Error: cannot compare 1x2 with 0
*** Where: <
*** Stack: mod 
```

`tuple!`

### 4.1 Absolute

There is no `abs` function in Red, but you can set a word to use the same data as absolute.
`abs: :absolute`


### 4.3 complement

`complement` does not accept numbers with decimal points in Red.
The following data types are accepted:

```red
ARGUMENTS:
     value        [logic! integer! bitset! typeset! binary!] 
```

### 4.6 negate

To negate a number, there must be no space between `-` and the number in Red.

Incorrect: `print - 2`

Correct: `print -2`


### 4.8 remainder

`//` is not an alias for `remainder` in Red.

From `help //`:

```red
Wrapper for MOD that handles errors like REMAINDER. Negligible values (compared to A and B) are rounded to zero. 
```

Examples must be:

`print remainder 11 2`

`print remainder 11.22.33 10`

`print remainder 11x22 2`


### 6.9 strict-not-equal

`strict-not-equal` is not defined in Red.

### 10.2 Math or number overflow

`1E+300 + 1E+400`

Returns `== 1.#INF` in Red.

### 10.3 Positive number required

`log-10 -1`

Returns `== 1.#NaN` in Red.

### 10.4 Cannot use operator on datatype! value

This is "type is not allowed here" error in Red:

```red
>> 10:30 + 1.2.3
*** Script Error: time! type is not allowed here
*** Where: +
*** Stack:
```

***
## <a name="chap12">Chapter 12 - Files

http://www.rebol.com/docs/core23/rebolcore-12.html

### 7.4 File Modification Date

There is no `modified?` function in Red.

### 7.5 Directory Information

There is no `info?` function in Red.

### 8.3 Renaming Directories and Files

`rename` does not exist in Red.

### 8.4 Deleting Directories and Files

`delete` does not take a `block!` in Red.

The following format will throw an error:
```red
delete [%file1 %file2 %file3]
```

Red does not have `delete/any`.

***

## <a name="chap15">Chapter 15 - Parsing

[Top](#top)

http://www.rebol.com/docs/core23/rebolcore-15.html

There are some key differences between Rebol `parse` and Reds `parse`.

The very first examples in the Rebol Guide will not work, as there is no `none` option for splitting strings.

[See parse blog entry for Red](http://www.red-lang.org/2013/11/041-introducing-parse.html)

In addition, I recommend this getting started article:
http://www.michaelsydenham.com/reds-parse-dialect/















