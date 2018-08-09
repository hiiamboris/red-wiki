Author: Peter WA Wood

These are the ideal capabilities for arrays in Red/System. They are provided for discussion. Some may be better provided through libraries than incorporated in the language.

1. Support of one-dimensional arrays (vectors) defaulting to zero-based indexing of a single type.
```red
arr: declare array [integer!] [10]		;; array of 10 integers, 0 - 9

a0: arr/0
i: 9
a9: arr/i
```
2. User definable ranges.
For cases where 1-based indexing is easier to handle than 0-based
```red
ar: declare array [char!] [1x10]		;; an array of 10 chars, 1 - 10
```
3. Ability to define array aliases.
So that arrays of arrays and structures can be built
```red
alar: alias array [integer!] [10] 

```
4. Ability for arrays of alias! types.
So that arrays of arrays or structs can be made.
```red
st!: alias struct! [
	r	[float!]
	g	[float!]
	b	[float!]
	alpha	[float!]
]

star: declare array [st!]

```
5. Length? function
To be able to loop through variable length arrays (see below).
```red
a: declare array [integer!] [10]
b: length? a								;; b = 10
```
6. REBOL block style literal value
```red
a: declare array [integer!] [4]
a: [1 2 3 4]
```
7. Adjustable size through push/pop/shift/unshift functions. 
For implementing lists, queues, etc.
```red
a: declare array [integer!] [4]
a: [1 2 3 4]
pop a							; returns 4, a = [1 2 3]
push a 5						; returns a, a = [1 2 3 5]
shift a							; returns 1, a = [2 3 5]
unshift a 0						; returns 
```
More REBOL-like function names could be used such as remove, append, take, and insert.
(Adding elements through assignment is not required).

8. Vector arithmetic for arrays of numeric types
For low-level graphics

Apply a calculation with a single value to all elements.
```red
a: declare array [4]
a: [1 2 3 4]
a: a * 3						; a = [3 6 9 12]
```
Calculation with of all elements in two arrays. (Controlled through alias! pseudo-type).
```red
at!: alias! array [4]
a: declare at!
b: declare at!
c: declare at!

a: [1 2 3 4]
b: [4 3 2 1]
c: a + b							; [5 5 5 5]
```
9. No equality test.
```red
a: declare array [2]
b: declare array [2]
a: [1 2 3 4]
b: [1 2 3 4]
a = b						;; compilation error

```
iArnold comments:

```red
@first I did not see a better way to add my comments but to edit them here at the bottom.
@second Good work Peter
@1 I prefer having array being base-1 indexed
@1 What about the arr/1 notation to retrieve array elements?
@2 This uses a pair type, could a tuple be an option too?
1.10 or 0.9 Or even 1..10 This rises the opportunity to give any base you want: 99..100
Even a strange -1..1 can be thinkable. Flexible indeed.
@5 length? for variable length arrays. So you define an array of say 4 elements and than keep pushing 
dataelements on the 'stack' even beyond the declared limit?
@8 a: [1 2 3] b: [4 5 6] sum: a + b ;; sum = [5 7 9]
and cat: append a b ;; cat = [1 2 3 4 5 6] ?
```

