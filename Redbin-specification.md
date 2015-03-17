Redbin aims at defining a binary format that accurately represents Red values stored in memory, while enabling fast loading (avoiding the parsing and validation stage of the text representation format). Redbin format is largely inspired by [REBin](http://www.rebol.com/article/0044.html).

The user interface for Redbin format access will be provided by `load/binary` and `mold/binary`. Underlying implementation _could_ use the codec sub-system, once available.

### Implementation constraints

* Base address in memory of Redbin data needs to be 64-bit aligned.

## Encoding format

The _default_ encoding format is optimized for decoding speed, while the _compact_ format requires a smaller storage space (at the expense of much slower decoding).

Values are stored in little-endian format.

Lexical conventions:

1. _Numbers in parens indicate the byte size of the field._

1. _Field names followed by a datatype name in a block are placeholders for a value of that datatype._

1. _Field names followed by equal sign have a fixed value._

Index:
[Header](#header)
[Global section](#Global section)
[Header](#header)
[Header](#header)
[Header](#header)
[Header](#header)

### Header
```
magic="REDBIN" (6), version=1 (1), flags (1)

flags (option is enabled if bit is set):
     bit0: compact mode
     bit1: compressed
     bit2-7: <reserved>
```

### Global section

TBD

### Padding
```
Default:
	type=0 (4)
Compact:
        n/a
```
This empty type slot is used to properly align some 64-bit values.

### Datatype!
```
Default:
	type=1 (4), value (4)
Compact:
        TBD
```

### Unset!
```
Default:
	type=2 (4)
Compact:
        TBD
```

### None!
```
Default:
	type=3 (4)
Compact:
        TBD
```

### Logic!
```
Default:
	type=4 (4)
Compact:
        TBD
```

### Block!
```
Default:
	type=5 (4), length (4), ...
Compact:
        TBD
```
The `length` field contains the number of values to be stored in the block. The block values are simply following the block definition, no separator or end delimiter is required.

### Paren!
```
Default:
	type=6 (4), length (4), ...
Compact:
        TBD
```
Same encoding rules as block!.

### String!
```
Default:
	type=7 (4), UCS=1|2|4 (1), length (3), data (UCS*length)
Compact:
        TBD
```
The `UCS` field indicates the encoding format of the string, only values of 1, 2 and 4 are valid. The `length` field contains the number of codepoints to be stored in the string, up to 16777215 codepoints (2^24 - 1) are supported. The string is encoded in UCS-1, UCS-2 or UCS-4 format. No NUL character is present, nor accounted for in the `length` field.

### File!
```
Default:
	type=8 (4), UCS=1|2|4 (1), length (3), data (UCS*length)
Compact:
        TBD
```
Same encoding rules as string!.

### Url!
```
Default:
	type=9 (4), UCS=1|2|4 (1), length (3), data (UCS*length)
Compact:
        TBD
```
Same encoding rules as string!.

### Char!
```
Default:
	type=10 (4), value (4)
Compact:
        TBD
```

### Integer!
```
Default:
	type=11 (4), value (4)
Compact:
        TBD
```

### Float!
```
Default:
	[padding=0 (4),] type=12 (4), value (8)
Compact:
        TBD
```
The optional padding field is added to properly align the `value` field offset to a 64-bit value.

### Word!
```
Default:
	type=15 (4), symbol (4), context (4), index (4)
Compact:
        TBD
```

### Set-word!
```
Default:
	type=16 (4), symbol (4), context (4), index (4)
Compact:
        TBD
```

### Lit-word!
```
Default:
	type=17 (4), symbol (4), context (4), index (4)
Compact:
        TBD
```

### Get-word!
```
Default:
	type=18 (4), symbol (4), context (4), index (4)
Compact:
        TBD
```

### Refinement!
```
Default:
	type=19 (4), symbol (4), context (4), index (4)
Compact:
        TBD
```

### Issue!
```
Default:
	type=20 (4), symbol (4)
Compact:
        TBD
```

### Native!
```
Default:
	type=21 (4), ID (4)
Compact:
        TBD
```
`ID` is an offset into the internal `natives/table` jump table.


### Action!
```
Default:
	type=22 (4), ID (4)
Compact:
        TBD
```
`ID` is an offset into the internal `actions/table` jump table.

### Op!
TDB

### Function!
```
Default:
	type=24 (4), spec [block!], body [block!]
Compact:
        TBD
```
`ID` is an offset into the internal `actions/table` jump table.