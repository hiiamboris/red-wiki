Red is a homoiconic language (Red is its own meta-language and own data-format) and an important paradigm it uses is “code is data”. 
A Red program is a sequence of Red values - everything is data before it’s evaluated. This makes one able to *express any task using syntax that best suits it*. The execution of a Red program is done by evaluating each of its constituent values in turn, according to the evaluation rules.
### Words
A special category of values is word. A word! is a symbolic value that can be used like a variable (the `!` at the end denotes a datatype). Red does not have identifiers nor keywords. Words do not store values, they point to values in some context – the global context by default.
Words are formed by one or more characters from the entire Unicode range, including punctuation characters from the ASCII subset: `! & ' * + - . < = > ? _ | ~`` 

Here are some valid words:

```
foo
foo+bar
Fahrenheit451 
C°
__ (two underscores)
```

Words can’t start with a digit and can’t contain any control characters, whitespace characters, and punctuation characters from the ASCII subset: `/ \ ^ , [ ] ( ) { } " # $ % @ : ;`

The presence of `! & ' * + - . < = > ? _ | ~`` in words leads to the implication that they must be delimited by spaces, tabs or \[]\(){}. In Python you can write:
```
a+b
1and a>b
```
In Red you always need to put spaces between words:
```
a + b
1 and a > b
```

Please note that Red is case insensitive.

### Evaluation order
At a semantic level, the Red program consist of expressions and not values. An expression groups one or more values, and may be formed in three ways: as an application of a (prefix) function, as an infix expression which uses an operator, or as a binding of a word to refer to a value.

Functions in Red always have a fixed number of arguments (fixed arity), as opposed to Python, where one can have default arguments and variable-length arguments. Functions are called by their name followed by the arguments – no need of parentheses nor commas.

```
print  “Hello, world!”
add 2 3
find "Red" #"e"
```

Operators are always binary operations, like `+` (addition), `-` (subtraction) and so on.

Evaluation of the operands of operators has precedence over function application and binding. There is no precedence between any two operators. This is different from Python, where the operators have different [precedence](https://docs.python.org/3/reference/expressions.html#operator-precedence)

```
2 + 2      ; evaluates to 4
2 + 3 * 4   ; evaluates to 20, not 14!
max 3 + 4 5   ; evaluates to 7
```

As you may have guessed, `;` starts a comment until the end of the line. 
Let’s take for example the following expression:
```square-root 4 + 5```
The operator `+` has precedence over the function `square-root` and that’s why Red first adds 5 to 4 and only then finds the square root of 9, resulting in 3.0.

Since the function arguments aren’t enclosed in parentheses, a programmer must know the arity of the functions. 

Evaluation order can be changed by the use of parentheses: 

```
2 + (3 * 4)    ; evaluates to 14
(length? "abcd") / 2
```

If we had written `length? "abcd" / 2`, it would have resulted in an error, because Red would first try to divide “abcd” by 2.

### [Datatypes](https://github.com/red/docs/blob/master/en/datatypes.adoc)

Red has a rich set of datatypes. Here are some types to start with:

* Integer! - 32-bit numbers with no decimal point.

1234, +1234, -1234, 60'000'000

* Float! - 64-bit positive or negative number that contains a decimal point.

+123.4, -123.4, 0042.0, 60'000'12'3.4

* logic! – Boolean values

true false, yes no, on off

* set-word! - Sets a reference to a value.

text: "Python and Red"

* char! - Unicode code points.

#"a", #"^C", #"^(esc)"

* string! - Sequence of Unicode code points (char! values) wrapped in quotes.

“Red”

Unlike “Python”, strings in Red are mutable. 
For  example, compare this Python code
```
>>> txt = "abcd"
>>> txt.upper()
'ABCD'
>>> txt
'abcd'
```
with Red:
```
>> txt: "abcd"
== "abcd"
>> uppercase txt
== "ABCD"
>> txt
== "ABCD"
```

Multiline strings are enclosed in {} and can contain double-quotes:
{This text is
split in "two" lines} 

* block! - Collections of data or code that can be evaluated at any point in time. Values and expressions in a block are not evaluated by default. This is one of the most versatile Red types.

[], [one 2 "three"], [print 1.23], [x + y], [dbl: func[x][2 * x]]

* paren! - Immediately evaluated block!. Evaluation can be suppressed by using quote before a paren value. Unquoted paren values will return the type of the last expression.

(1 2 3), (3 * 4), (x + 5)

Please note that if `x` doesn’t have a value in the current context, the last example will throw an error.

 * path! - Series of values delimited by slashes /. Limited in the types of values that they can contain – integers, words or parens.

buffer/1, a/b/c, data/(base + offs)

Path notation is used for indexing a block. Please note that Red uses 1-based indexing.
The following Python code
```
>>> mylist = [3,1,4,2]
>>> mylist[0]
3
```

Can be written in Red as follows:
```
>> mylist: [3 1 4 2]
== [3 1 4 2]
>> mylist/1
== 3
```

One can access the nested values in a block using as many levels of `/` as needed:

```
>> a: [1 [2 3] "456"]
== [1 [2 3] "456"]
>> a/1
== 1
>> a/2
== [2 3]
>> a/2/2
== 3
>> a/3/1
== #"4"
```

* map! - Associative array of key/value pairs (similar to Python's dictionary)

#( ), #(a: 1 b: “two”)

The keys can be any type of the following [typesets]( https://github.com/red/docs/blob/master/en/typesets.adoc): 
 [scalar!]( https://github.com/red/docs/blob/master/en/typesets.adoc#scalar), [all-word!]( https://github.com/red/docs/blob/master/en/typesets.adoc#all-word), [any-string!]( https://github.com/red/docs/blob/master/en/typesets.adoc#any-string)

* object! - Named or unnamed contexts that contain word: value pairs.
```
xy: make object! [
    x: 45
    y: 12
    mult: func[k][x + y * k]    
]
```
Please not that at this time it is not possible to extend an object with new word: value pairs.
The objects in Red are prototype-based, and not class-based. 
You can create a new object `xyz` using `xy` as a prototype and describe just the new pairs:

```
>> xyz: make xy [z: 1000]
== make object! [
    x: 45
    y: 12
    mult: func [k][x + y * k]
    z: 1000
]
```

* function! - user-defined functions. Functions have specification and body:

```x+y: function [x y][x + y]```

There are also other kinds of functions - func, does, has - that will be explained in more details in a section dedicated to functions.

* op! - Infix function of two arguments.

`+ - * / // % ^`

* refinement! - Refinement! values are symbolic values that are used as modifiers to functions or as extensions to objects, files, urls, or paths.


```
>> replace/all "Mississippi" #"i" #"e"
== "Messesseppe"
```

Without the `/all` refinement only the first "i" would be changed to "e".


* pair! - Two-dimensional coordinates (two integers separated by a `x`)


1x2, -5x0, -3x-25

The pair fields can be accessed by /x and /y refinments (or /1 and /2)
`+, -, *, /, %, //, add, subtract, multiply, divide, remainder, and mod` can be used with pair! values.


* date! - Calendar dates, relying on the Gregorian calendar.

28-03-2021, 28/Mar/2021, 28-March-2021, 2021-03-28

As you can see, different input formats for literal dates are accepted. 

The fields of any `date!` value can be accessed using path accessors - `/date`, `/year`, `/month`, `day` (or alternatively just `/1` `/2` `/3` `/4`) 

One can use addition and subtraction operations with date!, as well as with date! and integer!. Dates will be explored in a special section.

* tuple! - Three to twelve positive integers separated by decimal points. Used for representing RGB and RGBA color values, ip addresses, and version numbers. 

`255.255.255.0`

