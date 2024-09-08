This page is *mainly* intended to host links to open design questions so that less active community members may give their input to influence and bring closer the final outcome. Yet feel free to add also explanations of the current designs and arguments on design cornerstones that arise from time to time.

The page is not intended to mirror the [red/REP repo](https://github.com/red/REP/issues), only to be an addition to it.

It is preferable to keep these lists sorted by the impact of the question (from the most to the least influential).

# Open questions

#### Vector & matrix DSL design
- [Comment in REP/43](https://github.com/red/REP/issues/43#issuecomment-463335295) // remove this once it gets it's own REP page

#### Parse DSL: simplify `fail` rule to `[end skip]`, `break`/`reject` to return success/failure *from the loop (while/any/some)*, `break now` as an emergency exit from the loop, bring `also` into parsing
- [non-idiomatic behavior of `parse` failure rules #3478](https://github.com/red/red/issues/3478#issuecomment-406884362)

#### Parse DSL: rules with arguments
- [red/parse April 4, 2019 11:55 PM](https://rebol.tech/gitter.im/red/parse/2019/#msg5ca66f4c8148e555b24cd89b)

#### Core: open problems of the object design
- see links below [the explanation](https://github.com/red/red/wiki/[DOC]-Object-Notes#construct-vs-contextobject)

#### Core: efficient vector arithmetic
- [Adding a number to a vector changes the vector itself #2216](https://github.com/red/red/issues/2216)

#### VID DSL: automatically bind (literal only?) actor & reaction bodies to the face?
- [red/help March 13, 2019 10:17 PM](https://rebol.tech/gitter.im/red/help/2019/#msg5c89572c1c18c82b3c07190d)

#### Core: loop return values
- [Discussion in #3158](https://github.com/red/red/issues/3158)
- [Discussion in #2873](https://github.com/red/red/issues/2873)

#### Core: behavior of series access outside the data boundaries
- [red/bugs May 31, 2018 9:36 PM](https://rebol.tech/gitter.im/red/bugs/2018/#msg5b1040cb361a950a662f019b)

#### VID DSL: should it allow to override already defined actors (e.g. `base on-down [probe 1] [probe 2]`)?
- [red/help May 12, 2019 7:19 PM](https://rebol.tech/gitter.im/red/help/2019/#msg5cd847775a1d435d462cc06f)

#### Core: how to solve inelegancies and dangers of `error? try`, `attempt` and `catch` on *arbitrary* code?
- [The flaw of `error? try` approach #3755](https://github.com/red/red/issues/3755)

#### Web: how should cached versions of remote files be named (considering user-friendliness, pathname length, sanitization of invalid chars)?
- [Discussion in the PR 3124](https://github.com/red/red/pull/3124)

#### Core: should operators use the result from funcs with literal arguments? (if `f: func [:x][x * 2]`, should `f 1 + 2` be equivalent to `(f 1) + 2`?)
- [Gitter discussion, a few pages long](https://rebol.tech/gitter.im/red/help/2019/#msg5d8e1f5a66c8b4512228a09d): key points are [how it failed in R3](https://gitter.im/red/help?at=5d8fb75c086a72719e7d5354), [why do we need it?](https://gitter.im/red/help?at=5d8fbfd9290b8c354af1d571), [example how this can be applied](https://gitter.im/red/help?at=5d8f8f15290b8c354af058c1)
- [Related issue](https://github.com/red/red/issues/2622)

#### Core: should percent type allow scientific notation, and should it be constrained in range?
- [Gitter discussion, a few days long](https://rebol.tech/gitter.im/red/bugs/2020/#msg5e9866725706b414e1ceec2f)

#### VID DSL: should `panel` face draw a `text` facet on it?
- [PR #4073 that removes it on Windows as it was buggy](https://github.com/red/red/pull/4073)

#### Core: do we want `/deep` refinement for `take`?
- [red/bugs April 27, 2020 10:38 PM](https://rebol.tech/gitter.im/red/bugs/2020/#msg5ea734ae568e5258e48a9dca)

#### Introduce `single? map`, but not `last? map` ?
- [Gitter discussion](https://rebol.tech/gitter.im/red/red/2021/#msg605a2c463b9278255bcb3283)

# Historical questions & explanations

#### Core: Why do we have to copy literal series manually?
- [some arguments](https://rebol.tech/gitter.im/red/red/2022/#msg6317441a6837563d1cbeb44c), [begins here](https://gitter.im/red/red?at=6317294e99949962934fba9b)
- TL;DR: simpler to play in console, less automatic copy/deeps required for loaded code, but mostly just a historical choice

#### Core: Why `none`, `false`, `true` are not keywords in Red?
- [red/bugs January 25, 2022 8:44 PM](https://rebol.tech/gitter.im/red/bugs/2022/#msg61f03716742c3d4b21b5c7c9), [begins here](https://gitter.im/red/bugs?at=61f032dbd41a5853f96ac5c4)
- TL;DR: we may want these to be words for dialects, refinements, object fields and datatype uniformity of loaded external data

#### Core: Arguments on why paths evaluate picked items (so-called active accessors)
- [red/red February 28, 2019 9:46 PM](https://rebol.tech/gitter.im/red/red/2019/#msg5c782ca0c1cab53d6f53dd6d)

#### Core: Why `x` throws an error if `x` is unset (and more on rationale behind `unset` invention)
- [April 24, 2021 2:52 PM](https://rebol.tech/gitter.im/red/help/2021/#msg60840673b6a4714a29e4227d) (starts at [April 23, 2021 12:22 AM](https://gitter.im/red/help?at=6081e904b6a4714a29df402b) )
- [an old R3 discussion on whether unset is truthy/falsey/error](http://www.rebol.net/cgi-bin/r3blog.r?view=0207)

#### Core: Why loops do not make their words local to loop body, but rely on `function` to do that?
- https://github.com/red/red/issues/972#issuecomment-851476989

#### Core: How it came to such `(1, 2, 3)` point syntax?
- https://github.com/red/red/wiki/%5BNOTES%5D-Point-design-summary

#### Command line argument parsing rules
- [PR #3870 and references from there](https://github.com/red/red/pull/3870)

#### Core: Why word and a single-word path are different (despite the visual similarity)
- [reddit - on words vs paths confusion](https://www.reddit.com/r/redlang/comments/86kdwr/on_words_vs_paths_confusion/)

#### Core: :get-word function argument evaluation semantics: R2- or R3-like? (final?)
- [Calling a function passed as get argument inside another function does not work #1850](https://github.com/red/red/issues/1850)

#### Core: How to allow maps to have `none` values?
- [Design notes on removing keys from MAP! values](https://github.com/red/red/wiki/[NOTES]-Design-notes-on-removing-keys-from-MAP!-values)
- [Gitter discussion](https://rebol.tech/gitter.im/red/red/2019/#msg5ce6ae55b313d7231416163d) after `remove/key` PR was merged.

#### Core: How money datatype equality and comparison rules should work?
- [PR that summarizes the issue](https://github.com/red/red/pull/4455)
- [Gitter discussion, a few days long](https://rebol.tech/gitter.im/red/red/2020/#msg5e8ee18c38198d56a18ed4b7)

#### Core: What should empty `any []` and `all []` return?
- [Discussion in issue #4469](https://github.com/red/red/issues/4469#issuecomment-635450881)

# See also

https://github.com/red/red/wiki/[NOTES]-Design-Deliberations

https://github.com/red/red/wiki/[NOTES]-Design-notes-on-removing-keys-from-MAP!-values

https://github.com/red/red/wiki/[NOTES]-WITH-function-design

https://github.com/red/red/wiki/[LINKS]-JOIN-REJOIN-COMBINE-MERGE-EXTEND-CONCAT-design-ideas

https://github.com/red/red/wiki/[NOTES]-Trig-Functions-Behavior-and-Design

