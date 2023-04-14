### Overview
Red has 4 different functions that convert a value of any type to a `string!` value:
* `to-string`, _i.e._ application of the general type conversion function `to` with target type `string!`
* `form`, produces a human-readable representation of the value
* `mold`, produces a representation that mirrors the source code of the value
* `mold/all`, produces a representation that can be read back by `load` to yield the original value

### Current state and restrictions
* `mold/all` is not yet implemented and is currently equivalent to `mold`; this only makes a difference for the predefined values `none`, `true`, `false` and the `datatype!` names, which are `load`ed back as `word!` values, not as values of their type
* `to-string` is not defined for `unset!` and `none!` values, and yields an error

### Comparison of the three functions `to-string`, `form` and `mold`
All three functions yield identical results for values of the following types:
* `logic!`, `number!`, `pair!`, `tuple!`, `date!`, `time!`
* `url!`, `email!`, `image!`
* `word!`, `bitset!`, `typeset!`

For these values, the resulting string is identical to the source code for the value, regardless of which function is used.

For `lit-word!`, `get-word!`, `set-word!` and `any-path!` see [issue #3409](https://github.com/red/red/issues/3409).

Furthermore, the functions `to-string`  and `form` yield identical results on values of all types except `unset!`, `none!`, those in `any-list!` (`block!`, `paren!` and `hash!`) and `binary!`. Examples:
* `to-string ()` => _error_, `form ()` => `""`
* `to-string none` => _error_, `form none` => `"none"`
* `to-string [1 2 3]` => `"123"`, `form [1 2 3]` => `"1 2 3"`, similar for `paren!` and `hash!`
* `to-string #{313233}` => `"123"`, `form #{313233}` => `"#{313233}"`

For other types, `to-string` and `form` omit the "decoration" that characterizes literals of the types, thus _e.g._:
* `form #"a"` => `"a"`
* `form "abc"`=> `"abc"`
* `form %abc` => `"abc"`, similar for `tag!`
* `form #abc` => `"abc"`, similar for `refinement!`
* `form object [a: 1 b: 2]` => `"a: 1 b: 2"`, similar for `map!`, `vector!`
* `form integer!` => `"integer"`

For types in `any-function!`, only the function type is given by `to-string` and `form`, thus:
* `form :if` => `"?native?"`, similar for `action!`, `op!`, `function!` and `routine!`

### Complete table
This is found [here](https://github.com/red/red/wiki/%5BDOC%5D-Table-for-to-string-etc.) 
