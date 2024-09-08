# Red/System HowTo

This is a Red/System how-to page. Feel free to add complete Red/System functions or solutions involves Red/System (like routines).

Check [Guru Meditations](https://github.com/red/red/wiki/%5BDOC%5D-Guru-Meditations) for other Red/System notes and also [Red/System Language Specification](https://static.red-lang.org/red-system-specs.html).

## How to get function name

Even though functions don't have names in Redbol languages, here @9214 shows us how to get the word used to call a function, if any:

```red
Red [
    Date: 3-Aug-2020
    Note: "Compile in release mode."
    Author: "Vladimir Vasilyev @9214"
    Link: https://rebol.tech/gitter.im/red/red/2020/#msg5f276914a2be9049908a06bd
]

name?: routine [/local frame name body?][
    frame: stack/ctop
    name:  as red-value! none-value
    until [
        body?: frame/header and stack/FLAG_IN_FUNC <> 0
        frame: frame - 1
        if body? [
            name: as red-value! word/push* frame/header >> 8 and FFFFh
            break
        ]
        frame = stack/cbottom
    ]
    SET_RETURN(name)
]

foo: does [print ["My name is" name?]]

foo
probe name?
; == My name is foo
```

[Here](https://rebol.tech/gitter.im/red/red/2020/#msg5f271d5cb9bc40357bb25be8) you can find another version.

## How to convert `c-string!` to `red-string!`

```
Red [Note: "compile with -r flag"]

foo: routine [
    /local c-string red-string
][
    c-string: "this is a C string!"
    red-string: string/load c-string length? c-string UTF-8
    SET_RETURN(red-string)
]

print replace foo "C" "Red"
```
