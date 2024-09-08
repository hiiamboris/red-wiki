## This came up when discussing [[REP 0101 - For loop function]]

Original: [September 30, 2016 5:14 PM](https://rebol.tech/gitter.im/red/red/2016/#msg57ee5729d38f186520b3434a)

After writing all this, I realized this may be a design for a range construct? (mentioned in http://curecode.org/rebol3/ticket.rsp?id=1993)
The REP at the time seemed to have (from my viewpoint) 2 goals:

1. Cater for programmers from other languages
2. Design a range construct

Originally this was a criticism of the REP, but then my thoughts began to wander and become more abstract.. so here is a more concrete snapshot of it:

***
> I like the proposal overall, gregg has put in all the features that I'd mostly want. (Except looping over collections `map-each`, `foreach`,  multidimensional arrays, .. :tongue: ). That said, would I use it? My intuition is.. probably not.. except to use some of the features (like ranges, rebol `for`). I want to ignore the syntax `for` vs `loop`, `every`-- and talk about the semantics (is that the right word?) for the moment.
# Loops, ranges, comprehensions 
## Symantics
### Current workflow
I tend to to work with series functional programming style, directly working on the data / collection type if possible. I do use `loop` and sometimes `repeat`, but it's mostly `foreach` and `collect` if there already is data (`series`) to work on. I miss `map-each` and `fold` being built-in. Handling collections may be out of scope for this?
>> The idea is to support mainly numeric loop constructs. Working like foreach and forskip is also possible

> #### Advantages of a single `word!` for looping
I'll be easily able to refactor my code from `loop` to `repeat` to `for` without changing the syntax much (Hmm.. would that be as easy as I think it is?)
### Future workflow?
I'd like to talk about 2 languages which handles "looping" very much the way I like. They have quite different implementations tho
#### Julia
Is very similar to redbol. Has 2 [words](http://julia.readthedocs.io/en/latest/manual/control-flow/#man-loops) `for` and `while` and uses a range syntax `start:optional_step:end`. For is nicely combined with [comprehensions](http://julia.readthedocs.io/en/latest/manual/arrays/#comprehensions). It all works well for upto 2-3 dimensions. For higher ones you have to explicitly reshape
#### J
Is very much different. It transparently handles the cases for multiple dimensions. The basic operator for ranges is `i.` so for example, `i. 2 _5` gives
```red
4 3 2 1 0
9 8 7 6 5
```
It doesn't have explicit looping, but uses other simple operators to get the result you want. It is an extreme case of trying to simplify things (which I personally like)
## Use case
The motivation for this seems to be to ease adoption for people from other programming languages? I think @PeterWAWood and @iArnold raises good points. Would this help adoption like @greggirwin says or introduce more complexities? I'm more inclined to agree with Peter here. I myself am a bit conflicted. I'd still use `loop 5 [code]`, but perhaps `for` for most others, and others to operate on `series!`

# TODO
(Convert this to a range article?)
## Ranges
Carl Sassenrath: http://www.rebol.net/cgi-bin/r3blog.r?view=0058
Gregg Irwin: http://www.rebol.org/ml-display-message.r?m=rmlGGHC
### As a shortcut syntax
Multiple data types?
### Interval arithmetic
Complex intervals
## Loops / collections at a higher level
Catamorphisms anamorphisms..
```red
a -> [a]          ; create range/collection
[a] -> a          ; fold / reduce
a -> [a] -> a     ; map? (this can be optimized in some cases)
[a] -> a -> [a]   ; or is this map? if so, what is the previous?
```