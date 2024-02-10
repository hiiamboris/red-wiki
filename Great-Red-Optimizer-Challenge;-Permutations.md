# Permutation Challenge Chat

> Gregg Irwin @greggirwin Mar 16 19:01

We don't like answering surveys, at least I don't. But we LOVE writing code. I
recently went looking for my old permutation function, but couldn't find it.
Maybe I imagined writing it, or it was so bad I destroyed the evidence. So I did
a quick search or two, pulled a couple books off my shelf, and realized that
copying a C++ or Lisp model wasn't what I wanted to do. Having the basics in my
head, I tripped a few times doing my own, but it was good for my brain. Here it
is:

```
permutations: function [
    "Return a block containing all the permutations of the series"
    series [series!]
][
    drop: [head remove find copy series val]    ; used like a subroutine
    either 1 = length? series [series][
        collect [
            foreach val series [
                foreach p permutations do drop [
                    keep/only either any-block? series [
                        to series compose [(val) (p)]
                    ][
                        make series reduce [val p]
                    ]
                ]
            ]
        ]
    ]
]
print mold permutations [a b c]
print mold permutations 'a/b/c
print mold permutations quote (a b c)
print mold permutations "abc"
print mold permutations %abc
print mold permutations <abc>
```

I should probably have destroyed this one too. It doesn't feel particularly
elegant, might be more efficient, and produces at least two bad results from the
above samples. So why post it?

Because turning The Great Red Optimizer (that's you all) loose on it will surely
produce a better result than I can alone, and it will be fun to watch. Go
recursive, or with an interchange approach. Should we have a next/prev-
permutation approach? The recursive model is not efficient. And how useful are
permutations beyond a certain point? What are the use cases?

- Fix the known bugs.
- Consider what types are useful to permute.
- Decide if it should have limits imposed.
- Have fun thinking about it.


> Galen Ivanov @GalenIvanov 02:00

@greggirwin Thank you for the challenge! I already have a function n-
permutaton that returns the nth permutation of a series, zero indexed. So

```
>> n-permutation [a b c] 0
== [a b c]
>> n-permutation [a b c] 5
== [c b a]
```

The first permutation is the original arrangement, tha last one - the reversed
original arrangement. [Combinatorics](https://github.com/GalenIvanov/Combinatorics-Red/blob/master/combinatorics.red)

Here's a link to the article that explains the math behind the method I used:
[Representing a Permutation](https://code.jsoftware.com/wiki/Doc/Articles/Play121)

Too bad (because of the notation used) it's in J - a wonderful array language

> dsunanda @dsunanda 03:29

A permuation toy that does not have to precalculate all the permutations - it
can instantly return you the 1000 billionth permutation of the alphabet:

```
nth-permutation: func [
    array-in [block! string!]
    nth [integer!]
    /local
    fact array-mid array-out
][
    fact: func [ "Reverse factorial calculator"
    n-in [integer!]
    /local n-out nn c digit fact-n
][
    n-out: copy []
    c: n-in
    nn: 0
    while [c <> 0] [
        nn: nn + 1
        append n-out c // nn
        c: to-integer (c / nn)
    ]
    return n-out
]
    nth: max 0 absolute nth  ;; sanitize input
    array-mid: copy/deep array-in
    array-out: make array-in []

    fact-n: fact (nth - 1)
    while [(length? fact-n) < length? array-in] [append fact-n 0]
    foreach digit reverse fact-n [
        append array-out pick array-mid (digit + 1)
        remove at array-mid (digit + 1)
    ]
    return array-out
]
```

```
print nth-permutation [1] 1
print nth-permutation [1 2] 1
print nth-permutation [1 2] 2
print nth-permutation "abcdefghijklmnopqrstuvwxyz" 1'000'000'000
```

> Galen Ivanov @GalenIvanov 03:34

@dsunanda Great! My function doesn't calculate all the permutations too, just
the one in interest.

> dsunanda @dsunanda 03:39

@GalenIvanov Cool! We've taken slightly different approaches - you are using 0-
based indexing, and I've gone for 1-based.

> Galen Ivanov @GalenIvanov 03:46

@dsunanda Yes, 1-based is idiomatic for Red

> hiiamboris @hiiamboris 04:03

```
ith-perm: function [a i] [
    also r: make a n: length? a: copy a
    loop n [
        append/only r take skip a i // n
        i: to 1 i / n
        n: n - 1
    ]
]
```

(index likely does not align with the counting they used in J)


> Petr Krenzelok @pekr 04:15

Don't do that, now Gregg will write down his elaborate on that stuff and it is
going to be long. Like real long :-)


> dsunanda @dsunanda 04:19

@hiiamboris That is amazing! So compact.

Added challenge....Can you tweak it so the permutations are returned in
"natural" order? eg:

```
(ith-perm [1 2 3 4] 1) = [1 2 3 4]
```

> hiiamboris @hiiamboris 04:20

It's just 0-based right now. It kinda makes some sense that 0th permutation is
synonymous to do not permute. (OK, guilty - it also makes the code shorter :D)

> hiiamboris @hiiamboris 04:39

swap should be faster in practice but the order is less natural:

```
ith-perm: function [a i] [
    also a: copy a
    forall a [
        swap a skip a i // n: length? a
        i: to 1 i / n
    ]
]
```

> hiiamboris @hiiamboris 04:50

turning The Great Red Optimizer (that's you all) loose

@greggirwin using the above function, the rest of the code will be:

```
fac: function [n][m: #[0 1] any [m/:n m/:n: n * fac n - 1]]
all-perms: function [a] [
    also r: make [] n: fac length? a
    repeat i n [append/only r ith-perm a i - 1]
]
```

or 

```
all-perms: func [a] [
    map-each/only i fac length? a [ith-perm a i - 1]
]
```

if HOFs are allowed ;)

> Rebol2Red @Rebol2Red 10:57

Like a challenge which I can't solve? How about writing dynamic loops to use for permutations.

```
;-----------------------------------------------------------------------
; PERMUTATIONS (No duplicates per group. The groups are 1234 1234 1234)
;-----------------------------------------------------------------------
permutationsblock:  copy []
permutations: 0
repeat a 4 [
repeat b 4 [if (a = b) [continue]
repeat c 4 [if (c = b) or (c = a) [continue]
repeat d 4 [if (d = c) or (d = b) or (d = a) [continue]

    repeat e 4 [
    repeat f 4 [if (f = e) [continue]
    repeat g 4 [if (g = f) or (g = e) [continue]
    repeat h 4 [if (h = g) or (h = f) or (h = e) [continue]

        repeat i 4 [ 
        repeat j 4 [if (j = i) [continue]
        repeat k 4 [if (k = j) or (k = i) [continue]
        repeat l 4 [if (l = k) or (l = j) or (l = i) [continue]

            rec: copy ""
            append rec reduce [a b c d e f g h i j k l]
            append/only permutationsblock rec
            ;probe permutationsblock ; DO NOT USE THIS! BECAUSE OF PRINTING THIS LASTS FOREVER!
            permutations: permutations + 1

         ]]]]
    ]]]]
]]]]
;-----------------------------------------------------------------------
; print ["Permutations - no repetitions:" permutations]
;-----------------------------------------------------------------------
write/lines %permutations-no-repetitions-4x3.txt permutationsblock
;-----------------------------------------------------------------------
; wait 3 ; is this needed? (because i think that a slower systems needs time to write a file
;-----------------------------------------------------------------------
call "start notepad permutations-no-repetitions-4x3.txt"
```

I need dynamic loops or optimisation.

Note: The number of permutations is: 13824
