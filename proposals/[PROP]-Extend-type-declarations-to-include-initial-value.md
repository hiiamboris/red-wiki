Extend the type definition block to include an initial value in a declaration. Code examples:

	st: declare struct! [
		a		[integer! 99]
		b		[logic! false]
	]

If 'FUNCTION proposal accepted:

	f: function [
		i		[integer!]
	][
		finished?	[logic! false]
		moulded-int	[c-string! "          "]
	][
		; function code
	]

Or better with **default values of certain arguments**:

```rebol
f: function [
	i		[integer! 55]
][
	finished?	[logic! false]
	moulded-int	[c-string! "          "]
][
	; function code
]
```
(calling such function could be done maybe by the use of empty parentheses `f ()` to use the default value)