# User Defined Types (UDT):
There are 2 syntaxes: 
* (type-a) `type-name#value`
* (type-b) `type-name#{characters}`

Type-a cannot have `spaces`. Type-b can have `spaces`.

`type-name` should be simple to read and write:
* common the ASCII letters a-z
* case insensitive as the Red (a = A, b = B...)
* common word separators found in the ASCII: `-`, `_`

Examples:
* `name`
* `name-name2`
* `NameName` (same as `namename`)
* `name_name`


`spaces` are characters not visible on the screen. Explained more in [Whitespace character wiki](https://en.wikipedia.org/wiki/Whitespace_character). Examples:
* newline
* spaces (as space between `name1` and `name2` in the following example: `name1 name2`)
* tabs ([Vertical tabs # HTML wiki](https://en.wikipedia.org/wiki/Vertical_Tab#HTML))


`characters` are any valid UTF8 character (or other character encoding specified by the user). `characters`, of course, include `spaces` (defined above).


`value` (as in type-a `name#value`) is any valid `characters` (defined above) but ****without**** any `spaces` (defined above).  
For example, `name#foo` is valid value, `name#foo baz` is not valid value.

Using `{}` in the type-b syntax suggests user that it's a string. A type provider can parse string however s/he want it. Using blocks (`[]`) would restrict users.  
For example if the Red doesn't allow comma (`,`) in a word s/he cannot write `[a,b]`. `{a,b}` is valid.

Using one variable name for many, especially different, things may cause errors.  
User can redefine type but s/he shouldn't do this. We should warn him/her at least.    
Using above syntax you can create hue-saturation-value UDT (note the use of 'type-name' to specify the UDT name, here `type-name` = `HSV`):
* `HSV#360,1.0,1.0`   

and hue-saturation-lightness UDT (here `type-name` = `HSL`)
* `HSL#360,1.0,1.0`  


both UDTs are using the similar syntax (after `#` both UDTs use: 3 numbers, separated by comma `,`, the first number is the type of `integer!`, the second and the third are type of `float!`)


Type provider (programmer that implements a type) will parse only his/her types. For example `HSV#360,1.0,1.0`:
1) the Red split it into `m: #[type-name: "HSV" value: "360,1.0,1.0"]`
2) the Red will call `parser: parsers(m/type-name) background-color: parser/parse(m/value)`
3) `background-color` will be an object:

```red
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
`#["a" 42]` in the Ruby will be `{"a" => 42}`
1) The type provider split on spaces (`result: [{"a"} "=>" "42"]`)
2) The type provider will parse it (s/he check if `"a"` is valid key, s/he checks if next "symbol" is `=>`, s/he check if `42` is valid as value; it will store key - value pairs).  

It's possible to parse anything, even [the programming language that is made of spaces](https://en.wikipedia.org/wiki/Whitespace_(programming_language)).

Functions' spec should accept a type provided by programmer.
For example `f: function [color1 [HSV]] [print color/hue]` so when `f` is called `color1: HSV#222,0.5,0.5 f color1` it will check if the types in the spec and the types of an argument match (`HSV/type-name == color1/type-name`).

## Implementations:

- [by Nędza Darek](https://github.com/nedzadarek/red_custom_type_proposal)

# Dependent types:
Dependent type is (from [wiki](https://en.wikipedia.org/wiki/Dependent_type)) is
> a type whose definition depends on a value. A "pair of integers" is a type. A "pair of integers where the second is greater than the first" is a dependent type because of the dependence on the value. 

Using above syntaxes we can easily implement the dependent types.  
We should just extend the `function` (as the `function` in `foo: function [a] [print a]`).  
Common example of dependent type is adding 2 vectors (arrays of the same length). 
```red
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
Here, `sum` function depends on the same `length`es of the vectors.

`function` will:
1) get `vect1`'s `length` attribute and it tries to store it into `length1` (`length1: length`); `length1` is not defined so it's ok
2) get `vect2`'s `length` attribute and it tries to store it into `length1`; `length1` has already value so it will compare `length1` and `vect1/length`
3) do the body of the function
4) check the return value of the function with the `returns` 
* Is the type of it the `my-vector!`?
* Is the `length` equal the `length1`?

In case of `sum vect1 vect2` it will pass: `length` of the `vect1` and the `vect2` are the same (point 1 and 2), type of the return value is the same as in spec (`my-vector!`) (point 4a), `length` of the return value is the same as in the spec (point 4b).  
In case of `sum vect1 vect3` it won't pass: `length` are not the same (point 2)

## Implementations:

- [by Nędza Darek](https://github.com/nedzadarek/red_dependent_types_proposal)
- [by Rebolek](http://red.qyz.cz/dependent-types.html)
