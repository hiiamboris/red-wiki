Red is a functional language, although, for a performance reasons, functions are not closures by default:
```red
sum: function [n [integer!] return: [function!]] [
	function [x] [x + n]
]
add-5: sum 5
print add-5 10    ;this generates error: "context for n is not available"
```
It's because the `add-5` function is executed outside `sum` function's context, where a value for word `n` was stored.

You have to create closures by yourself. There are multitude of approaches to that. You can give it a fresh context with required values:
```red
sum: function [nn [integer!] return: [function!]] [
	context [
		n: nn
		return function [x] [n + x]
	]
]
```
... or just insert values instead words directly into function body:

```red
sum: function [nn [integer!] return: [function!]] [
	f: function [x] [n + x]
	change body-of :f nn
	:f
]
```

Here are some solutions from a community to generate closures in an elegant way:

"capture-by-value" closure generator: https://gist.github.com/dockimbel/79237c5454481bcb7d68105281b142a8

@dockimbel's more advanced, but still pretty simple model:

```red
closure: func [vars spec body][
    ; Don't have to reuse 'spec name; just saves a word.
    bind (body-of spec: func spec body) (context vars)
    :spec
]

gen3: closure [var: 0] [] [var: var + 1]
```

Original, simple model:

```red
closure: func [
    vars [block!] "Values to close over, in spec block format"
    spec [block!] "Function spec for closure func"
    body [block!] "Body of closure func; vars will be available"
][
    func spec bind body context vars
]
closed-fn: closure [var: 1] [n] [var: var + n]
closed-fn 1
closed-fn 2
?? closed-fn
```

And one, that supports **recursive** closures, by @hiiamboris:
```red
closure: func [vars spec body] [
	context compose/deep [(vars) return this: func spec [(body)]]
]
sum-range-with-5: closure [n: 5] [a] [either a <= 0 [n] [a + this (a - 1)]]    ;function refers to itself by 'this word
print sum-range-with-5 3    ;prints 11, which is 5 + (1 + 2 + 3)
```


