# Rationale
There had been discussions about gitter rooms. Gitter naturally organizes rooms based on github repos. Altho it does allow creating custom rooms, this does lead to confusion of which room is what, especially due to gitter's limitations:

1. No threaded conversations
2. Welcome message can't be displayed or linked to once dismissed
3. Renaming rooms not based on repos are impossible(?)
4. Cannot move messages from one room to the next
5. No enforceability of rules (how can you do this anyway?)

# Possible Solutions
1. [Gitter Topics](https://rebol.tech/gitter.im/gitterHQ/home) was supposed to alleviate this, but:
   1. it's still in [beta](https://rebol.tech/gitter.im/gitterHQ/topics/topic/58cac9e6f7f7d4810423d98f/when-will-topics-be-added-to-gitter-communities)
   2. not like threads in other chat software as you'd expect
2. Most other programming languages have a single room
3. Use another chat service(again? :p )
4. Re-organize rooms (which this is about :)

One can look at the problem of organizing from 3 different perspectives:

## Audience based
The only problem with this is there isn't a way to properly guide people to the relevant room.
Also, I've not found a way to view or link to the welcome message once it's been dismissed (who isn't likely to hastily dismiss a modal prompt anyway 😄)
1. Beginners to redbol `welcome`, also could be *learn*, *hello (world)*, only intro (no large code here, more Q&A, point to docs and SO)
2. Re(d)bol users `help`, also could be *code-help*, *code-review* (any question should have at least partially working code here, or snippets. Link to SO code review)
3. Redsystem users `red/system`?
4. Red-development `red/red` (design vs regressions?)
5. Red code snippets, libraries, demos, etc. `mezz`, also could be *code(-snippets)*, *pastebin*
6. Language based, e.g. `Chinese`

### Other groups of people
Red for programmers of X ( we have  `lisp`, why not others)? altho, I think we should have paradigms instead of languages: *functional*, *object-oriented*, *logic*, *ML*

## Topic based
1. +Dialects and parse. `dialecting`
2. ++Metaprogramming `lisp` room sometimes has discussions a bit like this
3. VID `red-guibranch`?
4. +Draw `graphics`?
5. +Red concepts `concepts` semantics, syntactically invisible things like *context*, theory, etc
6. Tests `tests`? currently dedicated to quicktest and tests in red system
7. Random `sandbox`
8. Bugs.. `bugs` regressions/design vs other ?
9. Data types and formats, currently we have lots of these `RIF`, `vector-datatype`, etc

### Other topics
@Geeky personally feels more topical rooms would be useful, except for the problem of introducing clutter of ever more rooms.
Could be further subdivided into the following:
- Red design & concepts
(editors and IDE, debugging, performance, tests)
- Red real world applications
(math, algebraic geometry, scientific programming, redcv and visual media)
- External topics that have a bigger scope than just being within red
(other programming languages, Programming paradigms, Integrations and ffi, data formats, api, parsers)
- Red marketing and outreach

## Repo based
This is how it is naturally done
1. Red `red/red`
2. Code `red/code`, could be `samples` or `examples` to avoid confusion with a room to
3. Red official docs `red/doc` need a separate topical room on other docs, documentation style guides, etc.. 

Note: `+` is room suggestions.

# Further considerations
- What I don't know is how to gently enforce rules.
Maybe a bot that responds differently according to room it's in.
- When does a room outgrow the Red organization?
Maybe when it needs it's own repo or is it's own standard library

Based on gitter chat [May 11, 2017 7:18 PM](https://rebol.tech/gitter.im/red/red/welcome/2017/#msg591472988a05641b1170bfde)