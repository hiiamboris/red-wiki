There are 2 syntaxes: (type-a) `name#value`, (type-b) `name#{value1 value2 ... valueN}`.

Type-a cannot have spaces. Type-b can have spaces.

User shouldn't redefine types. Warn them at least. Using above syntax you can have `HSV#360,1.0,1.0` and `HSL#360,1.0,1.0` - both are using the same maximum values (360, 1.0 and 1.0), 3 values (1-360, 0.0-1.0, 0.0-1.0) and they are separated by comma (`,`).

Type provider (programmer that implements a type) will parse only his/her types. For example `HSV#360,1.0,1.0`:
1) the Red split it into `m: #(type-name: "HSV" value: "360,1.0,1.0")`
2) the Red will call `parser: parsers(m/type-name) background-color: parser/parse(m/value)`
3) `background-color` will be an object:

```
construct [
    type-name: "HSV"
    hue: 360
    saturation: 1.0
    value: 1.0
    method: func [a] [...]
    ; other functions (methods)
]
```

Types has functions called methods (from the OOP terminology). So you can do `background-color/darker` to make it darker, `background-color/hue 42` to change the hue, `background-color + background-color` to add 2 colors (it might be just a syntactic sugar for `HSV/add background-color background-color` or `background-color/add background-color`).

As for examples of type-b (with spaces), lets take the old Ruby's hash (map in the Red):
`#("a" 42)` in the Ruby will be `{"a" => 42}`
1) The red split on spaces (`result: [{"a"} "=>" "42"]`)
2) The type provider will parse it (s/he check if `"a"` is valid key, s/he checks if next "symbol" is `=>`, s/he check if `42` is valid as value; it will store key - value pairs).

Functions' spec should accept a type provided by programmer.
For example `f: function [color1 [HSV]] [print color/hue]` so when `f` is called `color1: HSV#222,0.5,0.5 f color1` it will check if the types in the spec and the types of an argument match (`HSV/type-name == color1/type-name`).

Dependent types:
Using above syntaxes we should just extend `function`.
For example if we want to add 2 vectors but with the same length we could do something like this:
```
vect1: my-vector#{1 2 3} ; object [type-name: "my-vector" arr: [1 2 3] length: 3 ]
vect2: my-vector#{1 2 3} ; object [type-name: "my-vector" arr: [1 2 3] length: 3 ]
vect3: my-vector#{1 2 3 4 5} ; object [type-name: "my-vector" arr: [1 2 3 4 5] length: 5] 

sum: function [
  vect1 [my-vector! length1: length]
  vect2 [my-vector! length1: length]

  returns: [my-vector! length: length1]
][
  make my-vector! compose [
    type-name: "my-vector"
    length: (length1)
    arr: copy []
    (  
      repeat count length [append arr (vect1/arr/:count + vect2/arr/:count)]
    )
  ]
]
```
`function` will:
1) get `vect1`'s `length` attribute and it tries to store it into `length1` (`length1: length`); `length1` is not defined so it's ok
2) get `vect2`'s `length` attribute and it tries to store it into `length1`; `length1` has already value so it will compare `length1` and `vect1/length`
3) do the body of the function
4) check the return value of the function with the `returns` 
a) Is it `my-vector!` 
b) type? Is it `length` equal `length1`?

In case of `sum vect1 vect2` it will pass: `length` of the `vect1` and the `vect2` are the same (point 1 and 2), type of the return value is the same as in spec (`my-vector!`) (point 4a), `length` of the return value is the same as in the spec (point 4b).  
In case of `sum vect1 vect3` it won't pass: `length` are not the same (point 2)