### Lexer and Literal Format

The Red Lexer will accept all floating number even if they are greater/lesser than DOUBLE_MAX/DOUBLE_MIN.
```
red>> 1E345
== INF

red>> 1E-345
== 0.0
```
Possible literal format for NaN and INFs.

- Using built-in words
```
red>> NaN
== NaN

red>> -INF
== -INF
```
- Using serialized form
```
red>> #[NaN]
== NaN

red>> #[-INF]
== -INF
```
### Arithmetic Operations

According to the IEEE 754 standard, NaN and INFs are the results of certain arithmetic operations:
```
red>> 1.0 / INF
== 0.0

red>> 1.0 / 0.0
== INF

red>> -1.0 / 0.0
== -INF

red>> 0.000000001 / 0.0
== INF

red>> 0.0 / 0.0
== NaN

red>> 9999999.9 + INF
== INF

red>> 9999999.9 - INF
== -INF

red>> INF + INF
== INF

red>> INF - INF
== NaN

red>> INF * INF
== INF

red>> INF / INF
== NaN

red>> 0.0 * INF
== NaN
```

### Comparisions

Floating-point numbers are compared according to the IEEE 754 standard:

* Finite numbers are ordered in the usual manner.
* Positive zero is equal but not greater than negative zero.
* INF is equal to itself and greater than everything else except NaN.
* -INF is equal to itself and less then everything else except NaN.
* NaN is not equal to, not less than, and not greater than anything, including itself.
```
red>> NaN = NaN
== false

red>> same? NaN NaN
== true

red>> NaN <> NaN
== true

red>> NaN < NaN
== false

red>> NaN > NaN
== false

red>> [1 NaN] = [1 NaN]
== false
```
```
red>> INF < NaN
== false

red>> INF > NaN
== false
```
```
red>> -0.0 = 0.0
== true

red>> same? -0.0 0.0
== false
```