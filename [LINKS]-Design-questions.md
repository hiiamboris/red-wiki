This page is *mainly* intended to host links to open design questions so that less active community members may give their input to influence and bring closer the final outcome. Yet feel free to add also explanations of the current designs and arguments on design corner stones that arise from time to time.

The page is not intended to mirror the [red/REP repo](https://github.com/red/REP/issues), only to be an addition to it.

It is preferable to keep these lists sorted by the impact of the question (from the most to the least influential).

# Open questions

#### Vector & matrix DSL design
- [Comment in REP/43](https://github.com/red/REP/issues/43#issuecomment-463335295) // remove this once it gets it's own REP page

#### Parse DSL: simplify `fail` rule to `[end skip]`, `break`/`reject` to return success/failure *from the loop (while/any/some)*, `break now` as an emergency exit from the loop, bring `also` into parsing
- [non-idiomatic behavior of `parse` failure rules #3478](https://github.com/red/red/issues/3478#issuecomment-406884362)

#### Parse DSL: rules with arguments
- [red/parse April 4, 2019 11:55 PM](https://gitter.im/red/parse?at=5ca66f4c8148e555b24cd89b)

#### Core: open problems of the object design
- see links below [the explanation](https://github.com/red/red/wiki/[DOC]-Object-Notes#construct-vs-contextobject)

#### Core: efficient vector arithmetic
- [Adding a number to a vector changes the vector itself #2216](https://github.com/red/red/issues/2216)

#### VID DSL: automatically bind (literal only?) actor & reaction bodies to the face?
- [red/help March 13, 2019 10:17 PM](https://gitter.im/red/help?at=5c89572c1c18c82b3c07190d)

#### Core: behavior of series access outside the data boundaries
- [red/bugs May 31, 2018 9:36 PM](https://gitter.im/red/bugs?at=5b1040cb361a950a662f019b)

#### VID DSL: should it allow to override already defined actors (e.g. `base on-down [probe 1] [probe 2]`)?
- [red/help May 12, 2019 7:19 PM](https://gitter.im/red/help?at=5cd847775a1d435d462cc06f)

#### Core: how to solve inelegancies and dangers of `error? try`, `attempt` and `catch` on *arbitrary* code?
- [The flaw of `error? try` approach #3755](https://github.com/red/red/issues/3755)

#### Core: how to allow maps to have `none` values?
- [Design notes on removing keys from MAP! values](https://github.com/red/red/wiki/[NOTES]-Design-notes-on-removing-keys-from-MAP!-values)

# Historical questions & explanations

#### Arguments on why paths evaluate picked items (so-called active accessors)
- [red/red February 28, 2019 9:46 PM](https://gitter.im/red/red?at=5c782ca0c1cab53d6f53dd6d)

#### Command line argument parsing rules
- [PR #3870 and references from there](https://github.com/red/red/pull/3870)

#### Why word and a single-word path are different (despite the visual similarity)
- [reddit - on words vs paths confusion](https://www.reddit.com/r/redlang/comments/86kdwr/on_words_vs_paths_confusion/)

#### Core: :get-word function argument evaluation semantics: R2- or R3-like? (final?)
- [Calling a function passed as get argument inside another function does not work #1850](https://github.com/red/red/issues/1850)



