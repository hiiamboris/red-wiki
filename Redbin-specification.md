_Specification version 1_

Redbin is a binary format that accurately represents Red values stored in memory, while enabling fast loading (avoiding the parsing and validation stage of the text representation format). Redbin format is largely inspired by [REBin](http://www.rebol.com/article/0044.html). Redbin can encode binding information for words and can handle cycles in any-block! values.

The user interface for Redbin format access will be provided by `load/binary` and `mold/binary`. Underlying implementation _could_ use the codec sub-system, once available.

### Implementation constraints

* Base address in memory of Redbin data about to be loaded, needs to be 64-bit aligned.

## Encoding format

The _default_ encoding format is optimized for decoding speed, while the _compact_ format requires a smaller storage space (at the expense of much slower decoding).

Values are stored in little-endian format.

Lexical conventions:

1. _Numbers in parentheses indicate the byte size of the field._

1. _Field names followed by a datatype name in a block are place-holders for a value of that datatype._

1. _Field names followed by equal sign have a fixed value._


### Header
```
magic="REDBIN" (6), version=1 (1), flags (1), length (4), size (4)

flags (option is enabled if bit is set):
     bit0: compact mode
     bit1: compressed
     bit2: symbol table
     bit3-7: <reserved>

length : number of root records to load.
size   : size of records payload in bytes.
```
If compression is applied, the data following the header is the payload to be compressed. Compression algorithm choice is implementation-dependent.

### Symbol Table
The symbol table is following immediately the header data. It is optional and should only be used if words are present in the rest of the Redbin payload. The symbol table has two sections:

* a table of offsets to string representation of each symbol
* strings buffers, NUL-terminated and concatenated to each other

The position of a symbol in the table is its _index_ (zero-based), and it is used as reference for a symbol in contexts and words. The strings buffers section contains UTF-8 encoded strings with an optional padding at end to ensure 64-bit alignment. The offsets in the table are offsets in bytes from beginning of the strings buffers section to the referred string buffer.

Table encoding:
```
Default: length (4), size (4), offset1 (4), offset2 (4),...
Compact: TBD
```
`length` field contains the number of entries in the table. `size` field indicates the size of the string buffer in bytes (including optional tail padding bytes).

During the decoding process, these symbols are merged within Red's own symbol table and the offsets are replaced by the symbol ID value from Red table. That is, the symbol references in the Redbin records are an indirect reference to Red's internal symbol table entries used only during the loading process.

After the Symbol Table, Red values are stored as records in sequence with no special delimiter or end marker. The loaded values from root level are usually stored in a block! series.

## Records definitions

Each records starts with a 32-bit `header` field defined as:
```
* bit31    : new-line flag
* bit30    : no-values flag (for contexts)
* bit29    : stack? flag    (for contexts)
* bit28    : self? flag     (for contexts)
* bit27    : set? flag      (for words)
* bit26-16 : <reserved>
* bit15-8  : unit (used for encoding elements size in a series buffer)
* bit7-0   : type
```
Here follows the description of each individual record:

Index:
* [Padding](#padding)
* [Datatype!](#datatype)
* [Unset!](#unset)
* [None!](#none)
* [Logic!](#logic)
* [Block!](#block)
* [Paren!](#paren)
* [String!](#string)
* [File!](#file)
* [Url!](#url)
* [Char!](#char)
* [Integer!](#integer)
* [Float!](#float)
* [Context!](#context)
* [Word!](#word)
* [Set-word!](#set-word)
* [Lit-word!](#lit-word)
* [Get-word!](#get-word)
* [Refinement!](#refinement)
* [Issue!](#issue)
* [Native!](#native)
* [Action!](#action)
* [Op!](#op)
* [Function!](#function)
* [Path!](#path)
* [Lit-path!](#lit-path)
* [Set-path!](#set-path)
* [Get-path!](#get-path)
* [Bitset!](#bitset)
* [Point!](#point)
* [Object!](#object)
* [Typeset!](#typeset)
* [Error!](#error)
* [Vector!](#vector)
* [Pair!](#pair)
* [Percent!](#percent)
* [Tuple!](#tuple)
* [Map!](#map)
* [Binary!](#binary)
* [Reference!](#reference)

### Padding
```
Default: header (4)
Compact: n/a

header/type=0
```
This empty type slot is used to properly align 64-bit values.

### Datatype!
```
Default: header (4), value (4)
Compact: TBD

header/type=1
```

### Unset!
```
Default: header (4)
Compact: TBD

header/type=2
```

### None!
```
Default: header (4)
Compact: TBD

header/type=3
```

### Logic!
```
Default: header (4), value=0|1 (4)
Compact: TBD

header/type=4
```

### Block!
```
Default: header (4), head (4), length (4), ...
Compact: TBD

header/type=5
```
The `head` field indicates the offset of the block reference, using a zero-based integer. The `length` field contains the number of values to be stored in the block. The block values simply follow the block definition, no separator or end delimiter is required.

### Paren!
```
Default: header (4), head (4), length (4), ...
Compact: TBD

header/type=6
```
Same encoding rules as block!.

### String!
```
Default: header (4), head (4), length (4), data (unit*length) [, padding (1-3)]
Compact: TBD

header/type=7
header/unit=1|2|4
```
`head` field has same meaning as for blocks. The `unit` sub-field indicates the encoding format of the string, only values of 1, 2 and 4 are valid. The `length` field contains the number of codepoints to be stored in the string, up to 16777215 codepoints (2^24 - 1) are supported. The string is encoded in UCS-1, UCS-2 or UCS-4 format. No NUL character is present, nor accounted for in the `length` field. An optional tail padding of 1 to 3 NUL bytes can be present to align the end of the string! record with a 32-bit boundary.

### File!
```
Default: header (4), head (4), length (4), data (unit*length)
Compact: TBD

header/type=8
header/unit=1|2|4
```
Same encoding rules as string!.

### Url!
```
Default: header (4), head (4), length (4), data (unit*length)
Compact: TBD

header/type=9
header/unit=1|2|4
```
Same encoding rules as string!.

### Char!
```
Default: header (4), value (4)
Compact: TBD

header/type=10
```

### Integer!
```
Default: header (4), value (4)
Compact: TBD

header/type=11
```

### Float!
```
Default: [padding=0 (4),] header (4), value (8)
Compact: TBD

header/type=12
```
The optional padding field is added to properly align the `value` field offset to a 64-bit value.

### Context!
```
Default: header (4), length (4), symbol1 (4), symbol2 (4),..., value1 [any-type!], value2 [any-type!], ...
Compact: TBD

header/type=14
header/no-values=0|1
header/stack?=0|1
header/self?=0|1
```
Contexts are Red values used internally by some datatypes like function!, object! and derivative types. A context contains two consecutive tables, the first one is the list of word entries in the context represented as symbol references, the second is the associated values for each of the symbols in the first table. `length` field indicates the number of entries in the context. Context records can only exist at root level, they cannot be nested. If `no-values` flag is set, it means that there are no values following the symbols (empty context). If `stack?` flag is set, then the values are allocated on the stack instead of the heap memory. The `self?` flag is used to indicate that the context is able to handle a self-referencing word (`self` in objects).

### Word!
```
Default: header (4), symbol (4), context (4), index (4)
Compact: TBD

header/type=15
header/set?=0|1
```
The `context` field is an offset from the beginning of the records section in the Redbin file referring to a context! value. The context needs to be located before the word record in the Redbin records list. If `context` equals -1, it refers to global context.

If the `set?` field is defined, this record is followed by an [any-value!] record, and the word will need to be set to that value (in the right context) by the decoder. This forms a name/value couple allowing to encode words' values in an adhoc way, when providing a sequence of values for a given context is too expensive (mostly for name/value couples in global context).

### Set-word!
```
Default: header (4), symbol (4), context (4), index (4)
Compact: TBD

header/type=16
```
Same as word!.

### Lit-word!
```
Default: header (4), symbol (4), context (4), index (4)
Compact: TBD

header/type=17
```
Same as word!.

### Get-word!
```
Default: header (4), symbol (4), context (4), index (4)
Compact: TBD

header/type=18
```
Same as word!.

### Refinement!
```
Default: header (4), symbol (4), context (4), index (4)
Compact: TBD

header/type=19
```
Same as word!.

### Issue!
```
Default: header (4), symbol (4)
Compact: TBD

header/type=20
```

### Native!
```
Default: header (4), ID (4), spec [block!]
Compact: TBD

header/type=21
```
`ID` is an offset into the internal `natives/table` jump table.


### Action!
```
Default: header (4), ID (4), spec [block!]
Compact: TBD

header/type=22
```
`ID` is an offset into the internal `actions/table` jump table.

### Op!
```
Default: header (4), symbol (4), 
Compact: TBD

header/type=23
```
`symbol` representes the action, native or function name (only from global context) used as the source for that op! value. 


### Function!
```
Default: header (4), context [context!], spec [block!], body [block!], args [block!], obj-ctx [context!]
Compact: TBD

header/type=24
```

### Path!
```
Default: header (4), head (4), length (4), ...
Compact: TBD

header/type=25
```
Same encoding rules as block!.

### Lit-path!
```
Default: header (4), head (4), length (4), ...
Compact: TBD

header/type=26
```
Same encoding rules as block!.

### Set-path!
```
Default: header (4), head (4), length (4), ...
Compact: TBD

header/type=27
```
Same encoding rules as block!.

### Get-path!
```
Default: header (4), head (4), length (4), ...
Compact: TBD

header/type=28
```
Same encoding rules as block!.

### Bitset!
```
Default: header (4), length (4), bits (length)
Compact: TBD

header/type=30
```
The `length` fields indicates the number of bits stored, rounded to the upper multiple of 8. The bits are memory dumps of the bitset! series buffer. Byte order is preserved. `bits` field needs to be padded with enough NUL bytes to keep the next record 32-bit aligned.

### Point!
```
Default: header (4), x (4), y (4), z (4)
Compact: TBD

header/type=31
```

### Object!
```
Default: header (4), context [reference!], class-id (4), on-set-idx (4), on-set-arity (4)
Compact: TBD

header/type=32
```
The `on-set-idx` field indicates the offset of the `on-change*` in the context values table. The `on-set-arity` stores the arity of that function.

### Typeset!
```
Default: header (4), array1 (4), array2 (4), array3 (4)
Compact: TBD

header/type=33
```

### Error!
```
Default: header (4), context [reference!]
Compact: TBD

header/type=34
```

### Vector!
```
Default: header (4), head (4), length (4), values (unit*length)
Compact: TBD

header/type=35
header/unit=1|2|4|8
```
`unit` indicates the size of the vector element type size: 1, 2, 4 or 8 bytes. The `values` field holds the list of values. `values` needs to be padded with NUL bytes to align the next record to a 32-bit boundary (if `unit` is equal to 1 or 2).

### Pair!
```
Default: header (4), x (4), y (4)
Compact: TBD

header/type=37
```

### Percent!
```
Default: [padding=0 (4),] header (4), value (8)
Compact: TBD

header/type=38
```
The optional padding field is added to properly align the `value` field offset to a 64-bit value.

### Tuple!
```
Default: header (4), array1 (4), array2 (4), array3 (4)
Compact: TBD

header/type=39
```

### Map!
```
Default: header (4), length (4), ...
Compact: TBD

header/type=40
```
The `length` field contains the number of elements (keys + values) to be stored in the map. The map elements simply follow the length definition, no separator or end delimiter is required.

### Binary!
```
Default: header (4), head (4), length (4), ...
Compact: TBD

header/type=41
```
Same encoding rules as block!.

### Reference!
```
Default: header (4), count (4), index1 (4), index2 (4), ...
Compact: TBD

header/type=255
```
This special record type stores a reference to an already loaded value of type any-block! or object!. This makes it possible to store cycles in Redbin. The reference is created from a path into the loaded values (assuming that the root values are stored in a block). Each `index` field points to the series or object value to go into, until the last one is reached, pointing to the value to refer to. The `count` field indicates the number of indexes to go through. If one of the indexes has to be applied to an object, it refers to the corresponding object's field (0 => 1st field, 1 => 2nd field,...). All indexes are zero-based.