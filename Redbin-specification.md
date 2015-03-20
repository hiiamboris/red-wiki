_Specification version 1_

Redbin is a binary format that accurately represents Red values stored in memory, while enabling fast loading (avoiding the parsing and validation stage of the text representation format). Redbin format is largely inspired by [REBin](http://www.rebol.com/article/0044.html). Redbin can encode binding information for words and can handle cycles in any-block! values.

The user interface for Redbin format access will be provided by `load/binary` and `mold/binary`. Underlying implementation _could_ use the codec sub-system, once available.

### Implementation constraints

* Base address in memory of Redbin data about to be loaded, needs to be 64-bit aligned.

## Encoding format

The _default_ encoding format is optimized for decoding speed, while the _compact_ format requires a smaller storage space (at the expense of much slower decoding).

Values are stored in little-endian format.

Lexical conventions:

1. _Numbers in parens indicate the byte size of the field._

1. _Field names followed by a datatype name in a block are placeholders for a value of that datatype._

1. _Field names followed by equal sign have a fixed value._

### Header
```
magic="REDBIN" (6), version=1 (1), flags (1)

flags (option is enabled if bit is set):
     bit0: compact mode
     bit1: compressed
     bit2: symbol table
     bit3-7: <reserved>
```

### Symbol Table
The symbol table is following immediatly the header data. It is optional and should only be used if words are present in the rest of the Redbin payload. The symbol table has two sections:

* a table of offsets to string representation of each symbol
* strings buffers, NUL-terminated and concatenated to each other

The position of a symbol in the table is its _index_ (zero-based), and it is used as reference for a symbol in contexts and words. The strings buffers section contains UTF-8 encoded strings with an optional padding at end to ensure 64-bit alignment. The offsets in the table are offsets in bytes from beginning of the strings buffers section to the referred string buffer.

Table encoding:
```
Default: length (4), offset1 (4), offset2 (4),...
Compact: TBD
```
`length` field contains the number of entries in the table.

During the decoding process, these symbols are merged within the Red own symbol table and the offsets are replaced by the symbol ID value from Red table. The symbol references in the Redbin records become then an indirect reference to Red's internal symbol table entries.

After the Symbol Table, Red values are stored as records in sequence with no special delimiter or end marker. The loaded values from root level are usually stored in a block! series.

## Records definitions

Index:
* [Padding](#padding)
* [Datatype!](#datatype)
* [Unset!](#unset!)
* [None!](#none!)
* [Logic!](#logic!)
* [Block!](#block!)
* [Paren!](#paren!)
* [String!](#string!)
* [File!](#file!)
* [Url!](#url!)
* [Char!](#char!)
* [Integer!](#integer!)
* [Float!](#float!)
* [Context!](#context!)
* [Word!](#word!)
* [Set-word!](#set-word!)
* [Lit-word!](#lit-word!)
* [Get-word!](#get-word!)
* [Refinement!](#refinement!)
* [Issue!](#issue!)
* [Native!](#native!)
* [Action!](#action!)
* [Op!](#op!)
* [Function!](#function!)
* [Path!](#path!)
* [Lit-path!](#lit-path!)
* [Set-path!](#set-path!)
* [Get-path!](#get-path!)
* [Bitset!](#bitset!)
* [Point!](#point!)
* [Object!](#object!)
* [Typeset!](#typeset!)
* [Error!](#error!)
* [Vector!](#vector!)

### Padding
```
Default: type=0 (4)
Compact: n/a
```
This empty type slot is used to properly align some 64-bit values.

### Datatype!
```
Default: type=1 (4), value (4)
Compact: TBD
```

### Unset!
```
Default: type=2 (4)
Compact: TBD
```

### None!
```
Default: type=3 (4)
Compact: TBD
```

### Logic!
```
Default: type=4 (4)
Compact: TBD
```

### Block!
```
Default: type=5 (4), head (4), length (4), ...
Compact: TBD
```
The `head` field indicates the offset of the block reference, using a zero-based integer. The `length` field contains the number of values to be stored in the block. The block values are simply following the block definition, no separator or end delimiter is required.

### Paren!
```
Default: type=6 (4), head (4), length (4), ...
Compact: TBD
```
Same encoding rules as block!.

### String!
```
Default: type=7 (3), UCS=1|2|4 (1), head (4), length (4), data (UCS*length)
Compact: TBD
```
`head` field has same meaning as for blocks. The `UCS` field indicates the encoding format of the string, only values of 1, 2 and 4 are valid. The `length` field contains the number of codepoints to be stored in the string, up to 16777215 codepoints (2^24 - 1) are supported. The string is encoded in UCS-1, UCS-2 or UCS-4 format. No NUL character is present, nor accounted for in the `length` field.

### File!
```
Default: type=8 (3), UCS=1|2|4 (1), head (4), length (4), data (UCS*length)
Compact: TBD
```
Same encoding rules as string!.

### Url!
```
Default: type=9 (3), UCS=1|2|4 (1), head (4), length (4), data (UCS*length)
Compact: TBD
```
Same encoding rules as string!.

### Char!
```
Default: type=10 (4), value (4)
Compact: TBD
```

### Integer!
```
Default: type=11 (4), value (4)
Compact: TBD
```

### Float!
```
Default: [padding=0 (4),] type=12 (4), value (8)
Compact: TBD
```
The optional padding field is added to properly align the `value` field offset to a 64-bit value.

### Context!
```
Default: type=14 (4), length (4), symbol1 (4), symbol2 (4),..., value1 [any-type!], value2 [any-type!], ...
Compact: TBD
```
Contexts are Red values used internally by some datatypes like function!, object! and derivative types. A context contains two consecutive tables, the first one is the list of word entries in the context represented as symbol references, the second is the associated values for each of the symbols in the first table. `length` field indicates the number of entries in the context. Context records can only exist at root level, they cannot be nested.

### Word!
```
Default: type=15 (4), symbol (4), context (4), index (4)
Compact: TBD
```
The `context` field is an offset from the beginning of the records section in the Redbin file referring to a context! value. The context needs to be located before the word record in the Redbin records list.

### Set-word!
```
Default: type=16 (4), symbol (4), context (4), index (4)
Compact: TBD
```
Same as word!.

### Lit-word!
```
Default: type=17 (4), symbol (4), context (4), index (4)
Compact: TBD
```
Same as word!.

### Get-word!
```
Default: type=18 (4), symbol (4), context (4), index (4)
Compact: TBD
```
Same as word!.

### Refinement!
```
Default: type=19 (4), symbol (4), context (4), index (4)
Compact: TBD
```
Same as word!.

### Issue!
```
Default: type=20 (4), symbol (4)
Compact: TBD
```

### Native!
```
Default: type=21 (4), ID (4)
Compact: TBD
```
`ID` is an offset into the internal `natives/table` jump table.


### Action!
```
Default: type=22 (4), ID (4)
Compact: TBD
```
`ID` is an offset into the internal `actions/table` jump table.

### Op!
TDB

### Function!
```
Default: type=24 (4), context [context!], spec [block!], body [block!], args [block!], obj-ctx [context!]
Compact: TBD
```

### Path!
```
Default: type=25 (4), head (4), length (4), ...
Compact: TBD
```
Same encoding rules as block!.

### Lit-path!
```
Default: type=26 (4), head (4), length (4), ...
Compact: TBD
```
Same encoding rules as block!.

### Set-path!
```
Default: type=27 (4), head (4), length (4), ...
Compact: TBD
```
Same encoding rules as block!.

### Get-path!
```
Default: type=28 (4), head (4), length (4), ...
Compact: TBD
```
Same encoding rules as block!.

### Bitset!
```
Default: type=30 (4), length (4), bits (length)
Compact: TBD
```
The bits are memory dumps of the bitset! series buffer. Bytes order is preserved. `bits` field needs to be padded with enough NUL bytes to keep the next record 32-bit aligned.

### Point!
```
Default:  type=31 (4), x (4), y (4), z (4)
Compact: TBD
```

### Object!
```
Default: type=32 (4), context [reference!], class-id (4), on-set-idx (4), on-set-arity (4)
Compact: TBD
```
The `on-set-idx` field indicates the offset of the `on-change*` in the context values table. The `on-set-arity` stores the arity of that function.

### Typeset!
```
Default: type=33 (4), array1 (4), array2 (4), array3 (4)
Compact: TBD
```

### Error!
```
Default: type=34 (4), context [reference!]
Compact: TBD
```

### Vector!
```
Default: type=35 (3), unit (1), head (4), length (4), values (unit*length)
Compact: TBD
```
`unit` indicates the size of the vector element type size: 1, 2, 4 or 8 bytes. `values` field holds the list of values. The values needs to be padded with NUL bytes to align next record to 32-bit boundary (if `unit` is equal to 1 or 2).


### Reference!
```
Default: type=255 (4), count (4), index1 (4), index2 (4), ...
Compact: TBD
```
This special record type stores a reference to an already loaded value of type any-block! or object!. This makes possible to store cycles in Redbin. The reference is created from a path into the loaded values (assuming that the root values are stored in a block). Each `index` field points to the series or object value to go into, until the last one is reached, pointing to the value to refer to. The `count` field indicates the number of indexes to go through. If one of the indexes has to be applied to an object, it refers to the corresponding object's field (0 => 1st field, 1 => 2nd field,...). All indexes are zero-based.