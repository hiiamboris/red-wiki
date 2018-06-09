# Overview
Red has 4 different functions that convert a value of any type to a `string!` value:
* `to-string`, _i.e._ application of the general type conversion function `to` with target type `string!`
* `form`, produces a human-readable representation of the value
* `mold`, produces a representation that mirrors the source code of the value
* `mold/all`, produces a representation that can be read back by `load` to yield the original value

# Current state and restrictions
* `mold/all` is not yet implemented and is currently equivalent to `mold`; this only makes a difference for the predefined values `none`, `true`, `false` and the `datatype!` names, which are `load`ed back as `word!` values, not as values of their type
* `to-string` is not defined for `unset!` and `none!` values, and yields an error

# Comparison of the four functions
All four functions yield identical results for values of the following types:
* logic! number! pair! tuple! date! time!
* url! email! image!
* word! any-path! bitset! typeset!   
