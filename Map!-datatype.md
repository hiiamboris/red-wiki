###Abstract

A map represents an associative array of key/value pairs. It provides a fast read access (using an hashtable internally) and a convenient specific syntax. Unlike the `hash!` datatype, a map is not a series, so does not have a concept of offset or positions. Conceptually, map! datatype sits in between hash! and object! datatypes.

###Literal syntax
```
   #(<key> <value>...)

   <key>  : hashed key, accepted types are:
                 any-word!, any-string!, integer!, float!, char!

   <value> : any-type! value, except unset! value
```
###Creation syntax
```
   make map! <spec>

   <spec> : block of name/value pairs or integer
```
If the _spec_ argument is an integer, an empty map! is created with a pre-allocated number of slots (usually in order to populate the map dynamically).

<u>Notes</u>: 
* the map body or the spec block have to contain an **even** number of values or an error will be generated. 
* values are not _reduced_, so construction syntax is required for some special values, like logic!.

Examples:
```
#(a: 1 b: 2)
== #(
    a: 1
    b: 2
)

make map! [a 1 'b 2 "c" 3]
== #(
    a: 1
    b: 2
    "c" 3
)
```
If the key is of **any-word!** type, the key type is converted to **set-word!** in the map. Accessing words keys can be done using **word!** values, providing a **set-word!** key is not necessary.

<u>Notes</u>: 
* like hash! and block!, map! is <u>case-sensitive for storage</u>, but <u>case-insensitive for lookup</u> by default.
* if a `none` value is specified as value, the key will not be created (see "Deleting keys" section).

Another way to create a new map is to `copy` an existing one

###Retrieving values

Using paths:
```
    <map>/<key>
    get '<map>/<key>

    <map> : word referring to a map! value
    <key> : word key to select a value in the map
```

Using selecting actions:
``` 
    pick <map> <key>
    select <map> <key>

    <map> : map value
    <key> : any valid key value to a select a value in the map
```
All these read accesses are case-insensitive. In order to have a case-sensitive lookup, the `/case` refinement needs to be used where available:
```
    get/case '<map>/<key>
    select/case <map> <key>
```
Examples:
```
   m: #(Ab: 2 aB: 5 ab: 10)
   m/ab
   == 2
   select m 'aB
   == 2
   get/case 'm/aB
   == 5
   select/case m 'ab
   == 10
```

###Changing keys and values

Using paths:
```
    <map>/<key>: <value>
    set '<map>/<key> <value>

    <map>   : word referring to a map! value
    <key>   : word key to select a value in the map
    <value> : any value, except unset! value
```

Using modifying action:
``` 
    poke <map> <key> <value>

    <map> : map value
    <key> : any valid key value to a select a value in the map
```
Making bulk changes:
```
    append <map> <spec>

    <map>  : a map value
    <spec> : block of name/value pairs (one or more pairs)
```

All these write accesses are case-insensitive. In order to have a case-sensitive lookup, the `/case` refinement needs to be used where available:
```
    set/case '<map>/<key> <value>
```
`append` action can accept many keys at the same time, so it is convenient for bulk changes.

<u>Notes</u>: 
* setting a key that does not exist previously in the map, <u>will simply create it</u>.
* adding an existing key will change the key value and not add a new one (case-insensitive matching by default).

Examples:
```
   m: #(Ab: 2 aB: 5 ab: 10)
   m/ab: 3
   m
   == #(
      Ab: 3
      aB: 5
      ab: 10
   )

   poke m 'aB "hello"
   m
   == #(
      Ab: "hello"
      aB: 5
      ab: 10
   )

   set/case 'm/aB 0
   m
   == #(
      Ab: "hello"
      aB: 0
      ab: 10
   )
   set/case 'm/ab 192.168.0.1
   == #(
      Ab: "hello"
      aB: 0
      ab: 192.168.0.1
   )
   
   m: #(%cities.red 10)
   append m [%cities.red 99 %countries.red 7 %states.red 27]
   m
   == #(
	   %cities.red 99
	   %countries.red 7
	   %states.red 27
    )
```

###Deleting keys

In order to delete a key/value pair from a map, you simply set the key to `none` value using one of the available ways.

Example:
```
	m: #(a: 1 b 2 "c" 3 d: 99)
	m
	== #(
		a: 1
		b: 2
		"c" 3
		d: 99
	)
        m/b: none
	poke m "c" none
	append m [d #[none]]
	m
	== #(
	    a: 1
	)
```
<u>Note</u>: construction syntax is required in the above example in order to pass a `none!` value and not a `word!` value (just one way to construct the spec block needed there).

###Reflection

TBD