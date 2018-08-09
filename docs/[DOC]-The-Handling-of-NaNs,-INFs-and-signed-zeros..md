### Literal Format

The Literal format for NaN and INFs. 
The advantage of this format is it's able to keep the special values as floats values.
```red
red>> 1.#NaN
== 1.#NaN

red>> 1.#INF
== 1.#INF

red>> -1.#INF
== -1.#INF
```

The Red Lexer will accept all floating number even if they are greater/lesser than DOUBLE_MAX/DOUBLE_MIN.
```red
red>> 1E345
== 1.#INF

red>> 1E-345
== 0.0
```

### Arithmetic Operations

According to the IEEE 754 standard, NaN and INFs are the results of certain arithmetic operations:
```red
red>> 1.0 / 1.#INF
== 0.0

red>> 1.0 / 0.0
== 1.#INF

red>> -1.0 / 0.0
== -1.#INF

red>> 0.000000001 / 0.0
== 1.#INF

red>> 0.0 / 0.0
== 1.#NaN

red>> 9999999.9 + 1.#INF
== 1.#INF

red>> 9999999.9 - 1.#INF
== -1.#INF

red>> 1.#INF + 1.#INF
== 1.#INF

red>> 1.#INF - 1.#INF
== 1.#NaN

red>> 1.#INF * 1.#INF
== 1.#INF

red>> 1.#INF / 1.#INF
== 1.#NaN

red>> 0.0 * 1.#INF
== 1.#NaN
```

### Comparisons

Floating-point numbers are compared according to the IEEE 754 standard:

* Finite numbers are ordered in the usual manner.
* Positive zero is equal but not greater than negative zero.
* INF is equal to itself and greater than everything else except NaN.
* -INF is equal to itself and less then everything else except NaN.
* NaN is not equal to, not less than, and not greater than anything, including itself.

```red
red>> 1.#NaN = 1.#NaN
== false

red>> same? 1.#NaN 1.#NaN
== true

red>> 1.#NaN <> 1.#NaN
== true

red>> 1.#NaN < 1.#NaN
== false

red>> 1.#NaN > 1.#NaN
== false

red>> [1 1.#NaN] = [1 1.#NaN]
== false
```

```red
red>> 1.#INF < 1.#NaN
== false

red>> 1.#INF > 1.#NaN
== false
```

```red
red>> -0.0 = 0.0
== true

red>> same? -0.0 0.0
== false
```