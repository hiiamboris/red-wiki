## Rebol/Core    Chapter 3

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

`send luke@rebol.com page` can not be used.

### 11. Files

`print modified? %file.txt`

`modified?` is not defined in Red.

### 12. Networking

Currently, only HTTP is available in Red.




