Redbin aims at defining a binary format for accurately representing Red values stored in memory, while enabling quick loading (avoiding the parsing and validation stage of the text representation format).

### Implementation constraints

* Base address of loading Redbin data needs to be 64-bit aligned.

Numbers in parens indicate the byte size of the field.

### File header
```
REDBIN (6), version=1 (1), flags (1)

flags (option is enabled if bit is set):
     bit0: compact mode
     bit1: compressed
     bit2-7: <reserved>
```

### Integer!
```
Default:
	type (4), value (4)
Compact:
        TBD
```

### Char!
```
Default:
	type (4), value (4)
Compact:
        TBD
```

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
	type=5 (4), length (4)
Compact:
        TBD
```
