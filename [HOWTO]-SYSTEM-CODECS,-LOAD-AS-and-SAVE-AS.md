# Introduction

`system/codecs` allows defining encoders and decoders that can be used by `load` and `save` in addition to the default Red format.

This is a brief discussion of the current state of affairs and ideas for possible improvements. It might evolve into separate documentation (for people wanting to use `system/codecs`) and design document (for the Red team).

# Basic codec example

```
append system/codecs reduce [
    'name context [
        Title:     "Title for the codec"
        Name:      'name
        Mime-Type: [mime/type]
        Suffixes:  [%.suffix]
        encode: func [value [any-type!] where [file! url! none!]] [
            ; encode VALUE and either write it to WHERE or return it (typically as a STRING! or BINARY!)
            ; if you are writing it directly, return WHERE, otherwise return the encoded data
        ]
        decode: func [source [file! url! string! binary!]] [
            ; decode SOURCE and return the decoded data
            ; you don't really get URL! values here as LOAD will READ/BINARY them for you
            ; you always get the FILE! for files (so you always have to READ yourself)
        ]
    ]
]
```

# Comments

* `load` will always `read` `url!` values for you, but you have to `read` files in `decode`; this seems inconsistent; perhaps it should always `read/binary` then `decode` can decide if to convert the binary to string first

* with `save` you have the choice to let it `write` the data for you or not, though the mechanism to do so is a bit obscure; perhaps it should always write for you, and decide on using `/binary` or not depending on whether you return `string!` or `binary!`

* I suspect that the reason for the way it is currently implemented is to allow streaming encoders and decoders; but it would perhaps be better to have a simple common case where `load` and `save` do the work for you and have a flag to indicate if the codec supports streaming; you'd want to use ports for that anyway and not have the codecs deal with `file!` or `url!` directly