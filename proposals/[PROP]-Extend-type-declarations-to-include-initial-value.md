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
