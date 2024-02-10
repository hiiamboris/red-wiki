# Point design summary

This page is to add info to the [Subpixel GUI blog post announcement](https://www.red-lang.org/2023/08/subpixel-gui.html).
Decisions tend to be forgotten even by the authors, so it's best to write the reasoning down.

### Problem 1: pairs are integer-only, which limits their use, introduces UI glitches (see examples in the blog post), requires ugly workarounds (like adhoc scaling of Draw commands to achieve subpixel precision). There's a big enough demand for subpixel accuracy.

One example is inability to draw a 1px wide box: `line-width 1 box 0x0 10x10` will paint the area between box `(-0.5, -0.5) .. (10.5, 10.5)` and `(0.5, 0.5) .. (9.5, 9.5)`, which of course will blur the line, and if draw is clipped within `0x0 .. 10x10`, then it will also appear as if it had `line-width 0.5`. To work around this one requires to write `scale 0.5 0.5 [line-width 2 box 1x1 19x19]` which is boring, and what if box is drawn with a bitmap pen? Then the pen needs separate unscaling as well, making it a workaround for a workaround :)

### Problem 2: pairs (`2x4`) look good for integer quantities for which they were designed for, not so much (`2353.173x-0.000169172`) for fractions, totally bad (`1.#NaNx-1.#INF`) for special floats. Syntax doesn't scale to floats.

Here's a visual comparison of various point syntax ideas, mixed with `compose` to see how new syntax plays along normal parens:
```
compose [box (xy1 - '(-1.0 2.0 -3.0)) '(10.031879104 20.0 30.1703710821)]
compose [box (xy1 - @(-1.0 2.0 -3.0)) @(10.031879104 20.0 30.1703710821)]
compose [box (xy1 - 3#(-1.0 2.0 -3.0)) 3#(10.031879104 20.0 30.1703710821)]						;) explicit dimensionality is unnecessary
compose [box (xy1 - (-1.0 2.0 -3.0)) (10.031879104 20.0 30.1703710821)]							;) directly conflicts with paren! evaluation
compose [box (xy1 - (-1.0,2.0,-3.0)) (10.031879104,20.0,30.1703710821)]
compose [box (xy1 - (-1.0, 2.0, -3.0)) (10.031879104, 20.0, 30.1703710821)]
compose [box (xy1 - (-1.0x 2.0y 3.0z)) (10.031879104x 20.0y 30.1703710821z)]					;) like complex 1.0 + 2.0i, closer to unit! type
compose [box (xy1 - -1.0x2.0x-3.0) 10.031879104x20.0x30.1703710821]								;) pair-like
compose [box (xy1 - -1.0_x_2.0_x_-3.0) 10.031879104_x_20.0_x_30.1703710821]						;) pair-like Gregg variant
compose [box (xy1 - -1.0,2.0,-3.0) 10.031879104,20.0,30.1703710821]
compose [box (xy1 - -1.0'2.0'-3.0) 10.031879104'20.0'30.1703710821]
compose [box (xy1 - -1.0|2.0|-3.0) 10.031879104|20.0|30.1703710821]
compose [box (xy1 - -1.0*2.0*-3.0) 10.031879104*20.0*30.1703710821]
compose [box (xy1 - <-1.0 2.0 -3.0>) <10.031879104 20.0 30.1703710821>]							;) <1 2 3> < <3 4 5> -- not so great :/
compose [box (xy1 - /-1.0 2.0 -3.0/) /10.031879104 20.0 30.1703710821/]							;) using invalid for refinements syntax
compose [box (xy1 - \-1.0 2.0 -3.0\) \10.031879104 20.0 30.1703710821\]
compose [box (xy1 - `-1.0 2.0 -3.0`) `10.031879104 20.0 30.1703710821`]							;) backtick has much better uses than this
compose [box (xy1 - make point! [-1.0 2.0 -3.0]) make point! [10.031879104 20.0 30.1703710821]]	;) runtime construction only, no lexical form
compose [box (xy1 - (-1.0 , 2.0 , -3.0)) make point! [10.031879104 20.0 30.1703710821]]			;) ditto, comma as a dimension-adding operator
compose [box (xy1 - make point! [-1.0 2.0 -3.0]) #(point! 10.031879104 20.0 30.1703710821)]		;) with construction syntax
compose [box (xy1 - (-1.0 , 2.0 , -3.0)) #(point! 10.031879104 20.0 30.1703710821)]				;) ditto
compose [box (xy1 - (-1.0x + 2.0y - 3.0z)) #(point! 10.031879104 20.0 30.1703710821)]			;) orderless construction using unit! for axis
compose [box (xy1 + 1.0x - 2.0y + 3.0z)) #(point! 10.031879104 20.0 30.1703710821)]				;) ditto
```
The `(-1.0, 2.0, -3.0)` syntax was chosen for good readability and for being standard point notation in geometry.
Drawback is it visually collides with `paren!` syntax, however in practice I (hiiamboris) have found it to be not such a big issue.

### Problem 3: we have comma used in `float!` datatype, e.g. `1.2 = 1,2`.

We had to free the comma from `float!`, so like in many other languages lexer now only understands `1.2` as float. Otherwise `(1,2)` would be a float, but `(1, 2)` - a point, which would be very confusing.

Generally one should not mix locale-aware formatting with the language syntax, so "in my locale comma is a decimal separator" is a very weak argument here. Localized numbers are meant for end-user, not for the programmer.

### Problem 4: `pair!` name itself implies 2 components, while we'll need 3D and 4D quantities.

Fitting more than 2 dimensions into `pair!` would make it a misnomer, which is unacceptable, so `point` was chosen as a new datatype name. 

### Problem 5: `point!` is a good name and `(1, 2)` - a syntactical notation, but it's specific, not as general as `pair!`. Naming is hard.

E.g. some are against expressing `face/size` as `point!` because `(20, 40)` doesn't read as natural for sizes as `20x40` notation.

Some of us also have used pair as an integer range, but using `point2D!` for it or any arbitrary pair of float numbers is a stretch.

We haven't found a better (more general) name. There's [`tuple` or `tuplet`](https://en.wikipedia.org/wiki/Tuple) in math, but we already have a different `tuple!` datatype. Splitting it into `couple!` `triple!` `quadruple!` is possible, but would imply getting rid of integer-based `pair!` and `triple!` (latter is nowadays only used internally by Parse engine). `quaternion!` is the only name that has no conflicts.

### Problem 6: unification of all 2/3/4D quantities under single `point!` datatype vs datatype multitude of `pair!`, `triple!`, `point2D!`, `point3D!`, `point4D!`. What's best?

**What if we unify of all points into `point!` datatype?**

* con: we lose the ability to leverage type checking to determine point dimensionality, e.g. `func [p [point2D!]]` would need a full runtime check, e.g. `if 2 <> length? p [ERROR "(p) is too huge"]`
* con: in the same way, dialects can't react to `point2D!` or `point3D!` directly without an explicit length check
* pro: we can have shorter type specs for functions that support all points: instead of `[any-point!]` or `[point3D! point4D!]` just have `[point!]`
* pro: a bit lighter runtime, since it has to deal with less datatypes
* ???: we have to define math rules across different dimensions, for example `(1,2) / (3,4,5)` or `max (1,2) (3,4,5)`. Missing dimensions could be assumed as zero, however in `(1,2,0) / (3,4)` that would lead to `(0.3333333, 0.5, 1.#NaN)` right away, which may be more of a problem than a useful behavior: at some stage into complex 2D computation creeps in a 3D point, and all results get wrecked?

**What if we get rid of `pair!`?** Unlike `integer!` which is useful for bitwise arithmetic, `pair!` is not backed by such need.

* con: we lose direct lexical compatibility on this form with REBOL, same code will have to be adapted
* con: we lose the simple `2x4` syntax and will have to write more complex `(2,4)`
* con: won't be able to use it in dialects, where it may be useful in addition to points, as it has integer meaning
* pro: less datatypes to care for, so less cognitive load on the programmer analyzing expressions with mixed pairs and points
* pro: generalized names `couple!`, `triple!` and `quadruple!` become an available option instead of the more specific `point!`, however the `(1,2,3)` syntax is still point-specific
* pro: simpler type checks - just `[point2D!]` instead of `[pair! point2D!]` in places that choose to accept both 

We could leave `2x4` syntax and let it be converted by lexer into `(2,4)`. Has some benefits, but won't round-trip, losing some legacy compatibility. We also considered choosing molded point format based on whether it has fraction, but it becomes an unacceptable mess:
```
>> p: 0x0 loop 4 [print p: p + 0.5]
(0.5, 0.5)
1x1
(1.5, 1.5)
2x2
```

### Task: would it be possible to also use expressions in point syntax on top of literal numbers, as in the original geometry notation?

https://github.com/red/REP/issues/149 explores this