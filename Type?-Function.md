A type? function would allow improved type checking of arguments passed to typed functions. The following code provides an example use:

```
LTM-MP-int!: alias struct! [
	used			[integer!]
	alloc			[integer!]
	sign			[integer!]
	mp-digit		[byte-ptr!]
]

LTM-MP-init-multi: func [
	"Initalises multiple new mp-ints"
	[typed]
	count           [integer!]
	list            [typed-value!]				;; list of mp-ints 
	return:         [integer!]
	/local	
		i			[integer!]
		tmp-list	[typed-value!]		
][
	
	;; check that type of the mp-ints to be initalised
	tmp-list: list
	until [
		i: i + 1
		if tmp-list/type <> type? LTM-MP-int! [return LTM-MP-INVALID-ARGS]
		tmp-list: tmp-list + 1
		i = count
	]

.
.
.
]
```
