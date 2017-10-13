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

2.3.0   ==  tuple!

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
`if time > 10:30 [send jim news]` will produce an error.

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

REBOL`
```
if time > 12:30 [print "past noon"]
```

Red
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

`reform` and `remold` do not exist in Red.

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








