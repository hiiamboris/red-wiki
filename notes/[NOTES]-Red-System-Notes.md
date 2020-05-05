# Indexing model

Indexing in Red/System is 1-based and continuous; this means that `/1` gives a pointed value and` /0` gives a value before it.

# What do `#system` and `#system-global` do, when used in Red?

`#system` includes the argument block of Red/System code at current position in the generated R/S source code resulting from Red code compilation. `#system-global` works similarly, but places the argument code in the global space, outside of the Red runtime and user code contexts. Normally, you use routines to interface Red and R/S code, though those directives can come 
[handy](https://github.com/red/red/blob/master/environment/system.red#L359) in [some](https://github.com/red/red/blob/master/libRed/libRed.red#L16) special [cases](https://github.com/red/red/blob/master/environment/functions.red#L914).

# Literal Array Pointers

See: https://static.red-lang.org/red-system-specs.html#section-4.8.6

Literal array pointers are always defined as int-ptr! regardless of the content of the array (otherwise it wouldn't be able to access the size nor support mixed type arrays). So if you want to access, e.g., floats, you need to acquire a float pointer first:

```red
powers: [1.0 2.0 3.0]
floats: as float-ptr! powers
probe floats/1
```

In Red/System version1 'DOES is simply shorthand for func []. It could be optimised by:

1. Storing the address of the following statement.
2. Compiling the code in the 'DOES block with a branch to the stored return address.
3. Branching to the start of the code of the 'DOES block.

When writing libs it is very common to define unique "sub-types" of Red/System's built-in simple types (integer!, byte!, float!, etc). If these could be declared as aliases instead of pre-processors defines, the compiler would apply an additional level of type checking that would catch "sub-type" errors for the programmer.

Before:
```red
#define sub-type-a! integer!
#define sub-type-b! integer!

a: as sub-type-a! 0
b: as sub-type-b! 100

my-func: [
	a		[sub-type-a!]
][
	either a = 100 [clear-all-data] [add-to-data a]
]

my-func b
```

After:
```red
sub-type-a!: alias integer!
sub-type-b!: alias integer!

a: as sub-type-a! 0
b: as sub-type-b! 100

my-func: [
	a		[sub-type-a!]
][
	either a = 100 [clear-all-data] [add-to-data a]
]

my-func b				!!!!! compiler error
```

# Declare and use unique symbols

Author: Peter W A Wood

Being able to declare and use unique symbols would improve program readability over using defined constants. As word! is not a valid type in Red/System, lit-word syntax could be used for symbol literals. This feature would allow the following code: 

```red
#define okay 	0
#define error1	1
#define error2	2
#define error3	3

my-func: [
	i		[integer!]
 	return:		[integer!]
][
	if i > 0 [return okay]
	if i = 0 [return error1]
	if i > -100 [return error2]
	return error3
]

response: my-func my-int
switch response [
	okay 		[print "okay"]
	error1 		[print "error1"]
	error2 		[print "error2"]
	error3 		[print "error3"]
][
	
```
to be re-written as:
```red
my-func: [
	i		[integer!]
 	return:		[symbol!]
][
	if i > 0 [return 'okay]
	if i = 0 [return 'error1]
	if i > -100 [return 'error2]
	return 'error3
]

print my-func my-int					;; assuming print omits the '
```

# Compile Time Option to Redefine Functions

Author: Peter W A Wood

When writing automated tests it is often necessary to re-define existing functions so as to be able to predict the response from the function. The most obvious example is testing anything that depends on the date and time. Other examples include functions that make network calls.

Providing a compile time option to allow the re-definition of functions would help people test their code better.

# Additional struct! features

To give improved external interfacing capability and possible speed optimisations the following additional struct! features are requested:

* choice to align struct! members on suitable boundaries (eg 64-bit floats on 64-bit boundaries) [See issue #503] (https://github.com/dockimbel/Red/issues/503)
* bit fields
* unions
* arbitrary padding

# Forward declaring struct alias

It is not supported, as Red/System has a single-pass compiler.

Macros can be used to workaround that:

```red
#define relationship! [ struct! [ startNode [ node! ] endNode [ node! ] ] ]
node!: alias struct! [ nextRel [ relationship! ] ]
```

# Embedding #system in a Red function

When using `#system` inside a Red function, it is important to note that Red functions are compiled to Red/System code.
For that matter, the following code produces a compilation error:
```
Red []

red-fun: function [][
    #system [        

        f: function [][        
            print-line "doing something"
        ]        
        cpt: 0        
        f
    ]
]
```
Compiler complains about `cpt` not being declared. This is because `red-fun` is compiled to a Red/System function and `#system` contents are inlined inside the Red/System `red-fun` function.
This means the above code example is compiled to this Red/System code:
```
Red/System []

red-fun: function [][       
    f: function [][        
        print-line "doing something"
    ]        
    cpt: 0        
    f
]
```
Since Red/System functions need explicit declaration of local variables, `cpt` is unknown to the compiler in `red-fun`.

The correct way to embed a function within another with `#system` is:
```
Red []

#system [
    f: function [][
        print-line "doing something"
    ]
    cpt: 0
]
red-fun: function [][
    #system [f]
]
```
Here, `cpt` is inlined outside the functions which is correct for the compiler.

Note: as stated by Nenad Rakocevic:
> the R/S compiler will let you declare and run the nested function, though it is not an officially supported feature, so we might deprecate it for 1.0.

# Known problems

## either

`either` should not be used as a part of expression, for example this gives incorrect result (bug [#3620](https://github.com/red/red/issues/3620)):
```
print 1 * 1 + either 0 <> 0 [1][0]    ;-- should print "1", but prints a random integer on each run.
```