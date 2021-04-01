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

`1234, +1234, -1234, 60'000'000`

* Float! - 64-bit positive or negative number that contains a decimal point.

`+123.4, -123.4, 0042.0, 60'000'12'3.4`

* logic! – Boolean values

`true false, yes no, on off`

* set-word! - Sets a reference to a value.

`text: "Python and Red"`

* char! - Unicode code points.

`#"a", #"^C", #"^(esc)"`

* string! - Sequence of Unicode code points (char! values) wrapped in quotes.

`“Red”`

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
`{This text is
split in "two" lines}`

* block! - Collections of data or code that can be evaluated at any point in time. Values and expressions in a block are not evaluated by default. This is one of the most versatile Red types.

`[], [one 2 "three"], [print 1.23], [x + y], [dbl: func[x][2 * x]]`

* paren! - Immediately evaluated block!. Evaluation can be suppressed by using quote before a paren value. Unquoted paren values will return the type of the last expression.

`(1 2 3), (3 * 4), (x + 5)`

Please note that if `x` doesn’t have a value in the current context, the last example will throw an error.

 * path! - Series of values delimited by slashes /. Limited in the types of values that they can contain – integers, words or parens.

`buffer/1, a/b/c, data/(base + offs)`

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

`#( ), #(a: 1 b: “two”)`

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


`1x2, -5x0, -3x-25`

The pair fields can be accessed by /x and /y refinments (or /1 and /2)
`+, -, *, /, %, //, add, subtract, multiply, divide, remainder, and mod` can be used with pair! values.


* date! - Calendar dates, relying on the Gregorian calendar.

`28-03-2021, 28/Mar/2021, 28-March-2021, 2021-03-28`

As you can see, different input formats for literal dates are accepted. 

The fields of any `date!` value can be accessed using path accessors - `/date`, `/year`, `/month`, `day` (or alternatively just `/1` `/2` `/3` `/4`) 

One can use addition and subtraction operations with date!, as well as with date! and integer!. Dates will be explored in a special section.

* tuple! - Three to twelve positive integers separated by decimal points. Used for representing RGB and RGBA color values, ip addresses, and version numbers. 

`255.255.255.0`


### Blocks and series

A block is a set of values arranged in some order. They can represent collections of data or code that can be evaluated upon request. Blocks are a type of [series!](https://github.com/red/docs/blob/master/en/typesets.adoc#series) with no restriction on the type of values that can be referenced. A block, a string, a list, a URL, a path, an email, a file, a tag, a binary, a bitset, a port, a hash, an issue, and an image are all series and can be accessed and processed in the same way with the same small set of series functions

Blocks in Red are similar to Python’s lists, but don’t forget that blocks are not evaluated until it’s necessary. Compare these code snippets:

Python
```
>>> p_list=[2+3,5]
>>> p_list
[5, 5]
```

Red
```
>> red-block: [2 + 3 5]
== [2 + 3 5]
```
As you can see, red-block remains unchanged, while p_list is formed by the evaluated values of its constituents.

Blocks are created by enclosing values (separated by whitespaces) in square brackets `[ ]`
```
[1 2 3]
[42 6 * 7 “forty-two” forty two]
```

Except literally, blocks can be created at runtime using a `make` constructor: 

```
>> make block! 20
== []
```

The above code creates and empty block pre-allocated for 20 elements.


Block can also be created by converting other values:

```
>> msg: "send %reference.pdf to mail@site.com at 11:00"
== "send %reference.pdf to mail@site.com at 11:00"
>> type? msg
== string!
>> to block! msg
== [send %reference.pdf to mail@site.com at 11:00:00]`
```

Here `msg` is of string! type. When converted to a `block!`, each part of the string is converted to a Red value (of course if it represents  a valid Red value):

```
>> foreach value to block! msg[print [value  ":" type? value]]
send : word
reference.pdf : file
to : word
mail@site.com : email
at : word
11:00:00 : time
```

The above code iterates over the items of the block created from a string using `to` conversion and prints the value and its type.

Please note that `to` function (technically it’s an [`action!`]( https://github.com/red/docs/blob/master/en/datatypes/action.adoc) expects a datatype OR an example value to which to convert the given value. This means that instead of `block!` we can use any literal block, even`[]`:

```
>> to [] msg
== [send %reference.pdf to mail@site.com at 11:00:00]
```

Now that you know what a block is and how you create one, let’s try to access block’s items. Let’s work with ` data: [3 1 4 1 5 9]`.  The simplest way one can reference an item in a block is using the item’s index in the block. Unlike Python, Red uses 1-based indexing. So, to get the first item we use `path notation` and an integer index:

```
data/1
== 3
>> data/2
== 1
```

Alternatively, we can use `pick`:

```
>> pick data 3
== 4
```

Please note that in Red it’s not possible to use `path notation` to index a literal block (or series). It’s perfectly valid to write in Python:

```
>>> [2,3,1][2]
1
```
To achieve a similar behavior in red we use `pick`:

```
>> pick [2 3 1] 3
== 1
```

A useful feature of `pick` is the possibility to use a `logic!` value for the index. The `true` value refers to the first item in the block (series) and the `false` value – to the second item.

```
>> pick data 2 > 3
== 1
>> pick data 2 < 3
== 3
```

Speaking of first and second items of a block, Red has predefined functions for accessing the first 5 items of a series:

```
>> first data
== 3
>> second data
== 1
>> third data
== 4
>> fourth data
== 1
>> fifth data
== 5
```

Let’s consider another block of values: ` signal: [a 2 7 b 1 8 c 2 8] `. Here `a b c` are just `word!`s – that is they represent themselves until they 	have some value in some context. 

```
>> first signal
== a
```
So , the first item if `signal` is just `a`. 

```
>> type? first signal
== word!
```
If we try to get the value `a` refers to, we get an error:

```
>> get first signal
*** Script Error: a has no value
*** Where: get
*** Stack:  
```
However, if we assign `a` value in the current (global) context, the first item of `signal` will be referring to it:

```
>> a: "abc"
== "abc"
>> get first signal
== "abc"
```
Of what use are the words in a block? We can use them to mark positions in the block for an easy access:

```
== 7
>> signal/a
== 2
>> signal/b
== 1
>> signal/c
== 2
```

Alternatively, we can use `select` to find a value in a series and get the value after it:

```
>> select signal 'a
== 2
>> select signal 2
== 7
>>

Let’s try to navigate within a block/series. Our new block will be `b: [1 2.0 #"3" "four"]`

`head` returns a series at its first index. Please note – the entire series, not the element at that position.

```
>> b
== [1 2.0 #"3" "four"]
>> head b
== [1 2.0 #"3" "four"]
```

Similarly, there is `tail` that returns a series at the index after its last value.

```
>> tail b
== []
```

Here `[]` is an empty block – there are no elements in the series at its tail.

If we are interested in the elements of a series between its head and tail, we can use `next` to iterate over the series. `next` returns a series at the next index:

```
>> next b
== [2.0 #"3" "four"]
>>
```
Please be careful - `next` doesn’t update the series, that’s why you need to use a `set-word!` to re-assign it:

```
>> next b
== [2.0 #"3" "four"]
>> b
== [1 2.0 #"3" "four"]
>> b: next b
== [2.0 #"3" "four"]
>> b
== [2.0 #"3" "four"]
```

Let’s compare Red’s `next` to Python’s `next()` method. 

```
>>> a = [1,'2',[1,2,3]]
>>> a_it = iter(a)
>>> next(a_it)
1
>>> next(a_it)
'2'
>>> next(a_it)
[1, 2, 3]
```

Python’s next()` returns a single element and not the list. If at any point you convert the iterator to a list using `list(a_it)` or `[*a_it]`, the iterator is exhausted and a subsequent call to `next(a_it)` raises a `StopIteration` exception. 

We said that `head` refers to the series at its first index – index 1. We can check the current index of a series with `index?`

```
>> b
== [2.0 #"3" "four"]
>> index? b
== 2
>> head b
== [1 2.0 #"3" "four"]
>> index? head b
== 1
>> index? tail b
== 5
```
Don’t forget that `tail` returns the series at the index after its last item. So `index? tail b` returns one more than the length of `b`.

We can find the length of a series using `length?`:

```
>> length? b
== 4
```

We can check if a series is at its head (first index) or tail with `head?` and `tail?` respectively:

``
`>> b
== [1 2.0 #"3" "four"]
>> head? b
== true
>> b: next b
== [2.0 #"3" "four"]
>> head? b
== false
>> b: tail b
== []
>> tail? b
== true
```

We saw that we can go from head to tail in a series using `next`. Similarly, we can go backwards with `back`:

```
>> b
== [1 2.0 #"3" "four"]
>> tail b
== []
>> back tail b
== ["four"]
```
