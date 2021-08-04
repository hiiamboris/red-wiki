# Pointers

```red
Red/System [
    Title:   "Pointers"
    Author:  "François Jouen"
]

print ["Let's play with structures and pointers" lf]
print ["Create structure and pointer" lf]
;--First we create a structure as an alias (no memory allocation)
union: alias struct! [
    f       [float!]
    i       [integer!]
    str     [c-string!]
]

;--then a pointer to the structure
p-struct!: declare struct! [ptr [union]] ;--memory allocation 

;--now we have to allocate the structure
un: declare union
un/f: pi
un/i: 255
un/str: "Hello Red world"

;--and to create a pointer to the structure
p-struct!/ptr: un   ; on fait pointer sur 

;--verify structure initialization
print ["Structure address: " un lf]
print ["Pointer value:     " p-struct! lf]
print ["Nice, we are pointing to the structure!" lf]
print [p-struct!/ptr/f " " p-struct!/ptr/i " " p-struct!/ptr/str lf]

print ["Now, let's modify the pointer values" lf]
p-struct!/ptr/f: 0.0
p-struct!/ptr/i: 0
p-struct!/ptr/str: "Bye bye crual world!"

print ["Pointer values are changed" lf]
print [p-struct!/ptr/f " " p-struct!/ptr/i " " p-struct!/ptr/str  lf]
print ["And the structure too! " lf]
print [un/f " " un/i " " un/str lf]
```

Results:

```red
Let's play with structures and pointers
Create structure and pointer
Structure address: 000030D0
Pointer value:     000030C4
Nice, we are pointing to the structure!
3.141592653589793 255 Hello Red world
Now, let's modify the pointer values
Pointer values are changed
0.0 0 Bye bye crual world!
And the structure too! 
0.0 0 Bye bye crual world!
```

# Strings

```red
Red [
    Title:   "Strings"
    Author:  "ldci"
]

;--the way to share global Red words with Red/System code

s: "Hello Red"                ;--string variable
b: #{48656C6C6F20526564}    ;--same string as binary

;--Red/System routines
getString: routine [return: [string!]][
    as red-string! #get 's
]

getBinary: routine [return: [binary!]][
    as red-binary! #get 'b
]

;--Red code
print ["Test String: " getString]
append s " is amazing"
print ["Test String: " getString]
print ["Test Binary: " getBinary]
```

Results:

```red
Test String:  Hello Red
Test String:  Hello Red is amazing
Test Binary:  #{48656C6C6F20526564}
```

# Vectors (Arrays)

```red
Red [
    Title:   "Arrays"
    Author:  "ldci"
]

;-- Basic Red Code
arr1: [1 2 3 4 5 6 7 8 9 10]
arr2: [512.0 255.0 127.0 64.0 32.0 16.0 8.0 4.0 2.0 1.0]

Print  "Red Array"
n: length? arr1
i: 1
while [i <= n][
    print [i ": " arr1/:i]
    i: i + 1
]

;--Now with routines for integer array

readIntegerArray: routine [ 
        array [block!] 
        /local 
        i             [integer!] 
        int [        red-integer!] 
        value tail     [red-value!]
][
    print ["size: " block/rs-length? array lf]
    value: block/rs-head array
    tail: block/rs-tail array
    print ["value: " value lf]
    print ["Tail : " tail lf]
    i: 1
    while [value < tail][
        int: as red-integer! value
        print [i ": " int " " int/value lf]
        value: value + 1
        i: i + 1
    ]
]

;--and float array

readFArray: routine [
    array [block!] 
    /local 
    i             [integer!] 
    f             [red-float!] 
    value tail     [red-value!]
][
    print ["size: " block/rs-length? array lf]
    value: block/rs-head array
    tail: block/rs-tail array
    print ["value: " value lf]
    print ["Tail : " tail lf]
    i: 1
    while [value < tail][
        f: as red-float! value
        print [i ": " f " "  f/value lf]
        value: value + 1
        i: i + 1
    ]
]
print newline
print ["Integer array"]
readIntegerArray arr1
print newline
print ["Float array"]
readFArray arr2
```

```red
Red Array
1 : 1
2 : 2
3 : 3
4 : 4
5 : 5
6 : 6
7 : 7
8 : 8
9 : 9
10 : 10

Integer array
size: 10
value: 023EFD20
Tail : 023EFDC0
1: 023EFD20 1
2: 023EFD30 2
3: 023EFD40 3
4: 023EFD50 4
5: 023EFD60 5
6: 023EFD70 6
7: 023EFD80 7
8: 023EFD90 8
9: 023EFDA0 9
10: 023EFDB0 10

Float array
size: 10
value: 023EFDD8
Tail : 023EFE78
1: 023EFDD8 512
2: 023EFDE8 255
3: 023EFDF8 127
4: 023EFE08 64
5: 023EFE18 32
6: 023EFE28 16
7: 023EFE38 8
8: 023EFE48 4
9: 023EFE58 2
10: 023EFE68 1
```