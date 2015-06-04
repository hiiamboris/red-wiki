Literal syntax:
```
   #(<name> <value>...)

   <name>  : hashed key, accepted types are:
                 any-word!, any-string!, integer!, float!, char!

   <value> : any-type! except none!
```
Creating a map can be done using the literal form or using `make map! <spec>` where `<spec>` is a block of conforming values. The map body or the spec block have to contain an **even** number of values or an error will be generated. Examples:
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

Like hash! and block!, map! is case-sensitive for storage, but case-insensitive for lookup, by default.

Reading a value selected by a key:
```
<map>/<key>
pick <map> <key>
select <map> <key>
get '<map>/<key>
```