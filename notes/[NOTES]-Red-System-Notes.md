# What do `#system` and `#system-global` do, when used in Red?

`#system` includes the argument block of Red/System code at current position in the generated R/S source code resulting from Red code compilation. `#system-global` works similarly, but places the argument code in the global space, outside of the Red runtime and user code contexts. Normally, you use routines to interface Red and R/S code, though those directives can come 
[handy](https://github.com/red/red/blob/master/environment/system.red#L359) in [some](https://github.com/red/red/blob/master/libRed/libRed.red#L16) special [cases](https://github.com/red/red/blob/master/environment/functions.red#L914).

# Literal Array Pointers

See: https://static.red-lang.org/red-system-specs.html#section-4.8.6

Literal array pointers are always defined as int-ptr! regardless of the content of the array (otherwise it wouldn't be able to access the size nor support mixed type arrays). So if you want to access, e.g., floats, you need to acquire a float pointer first:

```
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
```
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
```
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

```
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
```
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