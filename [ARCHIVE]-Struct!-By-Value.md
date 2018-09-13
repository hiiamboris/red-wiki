Some C libraries return structures by value rather than by reference. [CSFML is a good example](https://github.com/LaurentGomila/CSFML/blob/master/include/SFML/Window/VideoMode.h). Having a feature that would allow accepting a structure as a value would make it easier to bind to such functions.

## HOWTO

See https://static.red-lang.org/red-system-specs-light.html#section-4.7.2
```
s2: declare struct! [
   a   [integer!]
   b   [c-string!]
   c   [struct! [d [integer!] e [float!]] value] ;-- here has a `value`
]
```
> In this case, the nested `struct c` storage space is reserved when allocating the space for the parent struct s2. The size of struct s2 is 20 bytes, while the size of s is 12 bytes.


## Example

See https://github.com/haolloyin/reds-json/blob/master/json.reds#L401
```
parse-object: func [
    v       [json-value!]
    return: [json-parse-result!]
    /local
        ret     [integer!]
        size    [integer!]
        key-ptr [bytes-ptr!]
        len-ptr [int-ptr!]
        target  [byte-ptr!]
        m       [json-member! value]    ;-- add `value` to avoid static allocate memory for `m`
        i       [integer!]
][
    ...
]
```

or see https://github.com/haolloyin/reds-json/blob/master/json.reds#L43
```
json-member!: alias struct! [
    key     [c-string!]
    klen    [integer!]
    val     [json-value! value]    ;-- nested a struct! `json-value!` as a value, not a pointer!
]
```