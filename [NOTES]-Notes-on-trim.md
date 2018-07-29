#### Gregg Irwin

Should trim's doc string say what the default is? It does in R2.
Ah, except that's only for strings. For blocks, all none! values are removed by default.

#### @accumulism

I am using red-063. trim is not the same as trim/head/tail for me.

;== trim messes with each line, and leaves a cr at the end
;== trim/head/tail does not

reduce [
    trim {hello
    world        asdf
    asdf


}
    trim/head/tail {hello
    world        asdf
    asdf


}
]

======

[
    {hello^/world^-^-asdf^/asdf^/} 
    "hello^/^-world^-^-asdf^/^-asdf"
]


#### @accumulism

they are the same for one-line strings; different for multi-line strings

#### Gregg Irwin

Interesting, and...needs documenting. It appears to be by design. Internally, the default delegates to a trim-head-tail func, but the internal logic there explicitly looks for the refinement flags and considers newlines. @gltewalt and @accumulism, let's make sure this gets noted somewhere. It's subtle.
It's possible, but I can't say, that the current behavior is in preparation for /auto support.