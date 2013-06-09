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