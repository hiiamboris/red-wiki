"capture-by-value" closure generator: https://gist.github.com/dockimbel/79237c5454481bcb7d68105281b142a8

@dockimbel's more advanced, but still pretty simple model:

```
closure: func [vars spec body][
    ; Don't have to reuse 'spec name; just saves a word.
    bind (body-of spec: func spec body) (context vars)
    :spec
]

gen3: closure [var: 0] [] [var: var + 1]
```

Original, simple model:

```
closure: func [
    vars [block!] "Values to close over, in spec block format"
    spec [block!] "Function spec for closure func"
    body [block!] "Body of closure func; vars will be available"
][
    ;func spec compose [(bind body context vars)]
    func spec bind body context vars
]
closed-fn: closure [var: 1] [n] [var: var + n]
closed-fn 1
closed-fn 2
?? closed-fn
```