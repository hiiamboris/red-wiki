There is a reason we have `to` and `make` rather than a single action that serves both purposes. The rules for conversion and the rules for construction can differ. That is not an inconsistency, it is a purposeful design choice, separating rules into these two categories in order to offer more features while keeping the meaning clear to users.

We don't have any principle of least surprise to abide by, as Red lives in its own abstraction layer. There isn't any mainstream language which comes close to it. What matters is internal consistency, not consistency with languages living at the opposite end of the abstraction scale (e.g. C).

# Logic and integers

In Red, all values are `true` except `false` and `none`. Thus:

```red
>> to logic! 1
== true
>> to logic! 0
== true
```

is the expected result. Consequently, `true` (and by extension `false`), cannot be mapped back to an integer value, so conversion from `logic!` to `integer!` is an error. For the sake of boolean logic support, we also provide a mechanism to construct logic values from 1 and 0, using `make`. That is `make logic! <integer>` is allowed. For consistency and to reduce confusion, `make integer! <logic>` could throw an error too. Though that would remove a feature and introduce an arbitrary incompatibility with Rebol3.

Current behaviors:

```red
>> make integer! false
== 0
>> make integer! true
== 1
>> to integer! false
*** Script Error: cannot MAKE/TO integer! from: false
*** Where: to
*** Stack:  
>> to integer! true
*** Script Error: cannot MAKE/TO integer! from: true
*** Where: to
*** Stack:  
>> 
>> make logic! 0
== false
>> make logic! 1
== true
>> to logic! 0
== true
>> to logic! 1
== true
```

In a more concise way:

```red
 MAKE	    TO
------	  --------
T -> 1	  T -> n/a
F -> 0	  F -> n/a

 MAKE	    TO
------	  ------
1 -> T	  1 -> T
0 -> F	  0 -> T
```

So we can say that `make` relyies on boolean logic rules for constructing those values, while `to` relies on Red semantics for its behavior.