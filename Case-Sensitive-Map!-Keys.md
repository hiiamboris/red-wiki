Currently, the default handling of values of the `word!`, `path!` and `string!` types is on a case-preserving, case-insensitive basis within Red. This value consistency allows consistent function behaviour.

Applying this consistently to the upcoming `map!` datatype implies that it's keys of the `word!`, `path!` and `string!` datatypes would be case-insensitive. Unfortunately, this behaviour threatens the utility of `map!`, especially when using `string!` keys.

More often than not, associative arrays need to work with data using case-sensitive `string!` keys.
Some examples of data which must use case-sensitive keys, are:
* base-64 coded `string!`s
* any case-sensitive text data
* data from many file formats (eg. XML)
* filenames are case-sensitive in most file systems
* query strings in URLs
* usernames and passwords
* code or data from case-sensitive languages (eg. bash)
* tags
* ***** PLEASE INSERT OTHER IMPORTANT EXAMPLES HERE *****
* etc., etc., etc.,


Currently, Rebol 3's `map!` can't handle such real-world data because its keys are case-insensitive.
A workaround which has often been suggested to those trying to use Rebol 3's `map!`, is to convert `string!` values to `binary!` before inserting them as keys or looking them up. This hack is:
* extremely awkward to write in code
* difficult to debug (since one can't easily “see” what text their keys contain)
* too verbose
* less efficient (`map!`s are often used for speed)

Conversely, one could argue that if `map!`'s `string!` keys were case-sensitive by default, the workaround for dealing with case-insensitive data would be to use `lowercase` which would have all but one (the second) of the above problems. However, the last efficiency problem is there either way when dealing with case-insensitive data. `map!` can be used most efficiently on case-sensitive data, but only if it is case-sensitive by default.

Listed below are some ideas for providing case-sensitive `map!` keys. `word!`s, `string!`s and `char!`s are discussed for simplicity, but other datatypes (such as `file!`) will also need to be considered.


### Ideas

#### 1
**Throughout the entire Red language, all datatypes (including `word!`s, `string!`s and `char!`s) are case-sensitive.**

Pros:
* Provides full consistency.
* Provides full utility.
* Easiest to implement overall.
* Most efficient (folding is used only when specified).
* No need to choose between ANSII and unicode folding of words.
* The complexity of unicode doesn't threaten the consistency in the behaviour of identifying words.

Cons:
* It's quite a radical change from Rebol, and may receive too much resistance from the already existing community.
* Would mean that systems that generated file names or URLs to describe Red source would face problems in case-insensitive filesystems, or things like HTML anchors *(which are case insensitive, so if you tried to distinguish `http://help.red-lang.org/append#only` from `http://help.red-lang.org/append#ONLY` you cannot)*.  This is a real problem faced by Doxygen in generating filenames and URLs for C...if **FoOoBAR** and **fooObar** are semantically distinct, then tricks would needed to generate escaped identifiers with things like **-fo-o-b-a-r** vs **foo-obar**.  *(This is actually just a epicycle of the general problems with distinction you would face in describing the names over a Skype call.)*  Being more permissive about legal characters in words, Red would be harder to invent escape strategies for than C.
* Once case-sensitivity becomes meaningful for words, different modules by different authors would adopt different policies.  Some will try to encode data or meaning into casing of the same words *(`Foo` might be an object type, while `foo` is a variable, and `FOO` something else entirely...modules?  Private vs. public members?  Etc.)*  The current rarity and irrelevance of case would begin to shift, so that the arguments of why it "doesn't-matter-because-typos-are-rare" will become that the problem is precisely because they are *not* typos.  If you give people the "feature" they will use it whether you encourage it or not...which could (<sub>would</sub>) be detrimental to the ecology and unique written-language-style goals of the language.

Note:
* Most modern languages are like this now.
Scheme effectively went through this change in the revision from R<sup>5</sup>RS to R<sup>6</sup>RS. In this case, however, the community seemed to request it.
http://www.r6rs.org/final/html/r6rs-rationale/r6rs-rationale-Z-H-2.html#node_toc_node_sec_4.1.2
* May need to introduce something like `~` from [#2](#2) below.
* `select`'s `/case` refinement would need to be replaced with something like `/relaxed`, etc.
* What new word/s or refinement/s would have to be added to `parse`? Should such be already added anyway?
* So far, most who prefer this option seem to do so on the grounds of consistency and easier implementation.
A question to ask here, is “Is there anyone in the community who actually wants case-sensitive words because they miss them from other languages and want to use them?”
* So far, most who oppose this option seem to do so based on a fear of a greater opportunity to make typos. In my opinion, this is an irrational fear which is rarely complained about in practice by those using case-sensitive languages. Such typos would be most likely to occur when using camel-case, which from what I've seen, doesn't seem to be a Rebol convention. Most seem to type all in lowercase anyway.
* Personally, if Red were a brand new language and Rebol didn't exist, then full case-sensitivity would be my first vote, without hesitation. However, because I appreciate the history behind Red, [#2](#2) gets my first vote as seeming slightly more appropriate.


#### 2
**Throughout the entire Red language, all `word!`s are case-insensitive but `string!`s and `char!`s are case-sensitive.**

This follows from the observation that:
* Most of the arguments for case-insensitivity seem to be for `word!`s.
* Most of the arguments for case-sensitivity seem to be for `string!`s.

One possible way to achieve this might be if `=` were strict for `string!`s and relaxed for `word!`s. Then `==` would be strict for both, and something like `~` would be relaxed for both. Then, for example, `select` would use `=`, `select/case` (or `select/strict`) would use `==`, while something like `select/relaxed` could use `~`.
This could be seen as either more or less radical than [#1](#1). It could also be seen as either more or less in line with what Rebol currently is than [#1](#1).

Most of the arguments for both `word!`s and `string!`s having the same case sensitivity are from consistency. However, consider:

* `char!`s are already case-sensitive in Red (already breaking this definition of consistency). This way, `word!`s are the exception instead of `char!`s.
* `string!`s are made up of case-sensitive `char!`s.
* `word!`s are not made up of `char!`s.
* `string!`s can be converted to `binary!`, which is a case-sensitive operation.
* `word's!`s cannot be converted to `binary!` (well, they can if they're converted to `string!` first).
* `uppercase` and `lowercase` operate on `string!`s.
* `uppercase` and `lowercase` do not operate on `word!`s.
* A `string!` is a `series!`.
* A `word!` is not a `series!`.
* `word!`s and `string!`s are different datatypes with different purposes.
* `'a = "a"` is `false`.

Note:
* Something like `~` might be useful for `char!`s anyway.
* The `map!` problem may simply be a symptom of a deeper problem with trying to stay consistent with a flawed system. I personally think this proposal is a more powerful system to stay consistent with.
Perhaps `hash!`, `block!` and others can take advantage of case-sensitive `string!`s.
* New words and refinements would probably need to be added to `parse`.
* Personal note: This is the behaviour I expected when I first used Rebol, initially thinking `=` had a bug until I discovered `==`.


#### 3
**All keys in `map`! are the case-sensitive exception.**

Pros:
* Easiest change to implement in short term.
* Easy exception to teach if breaking consistency.

Cons:
* Breaks consistency, especially problematic for `word!` keys.


#### 4
**`map!`'s `string!` and `char!` keys are the case-sensitive exception.**

This is a compromise to make `word!`s consistent, but not `string!`s and `char!`s. This breaks consistency, but only for `string!` keys. The advantage this has over [#3](#3) is that `word!`s are the most important datatype to be consistent in the language, and least necessary datatype to be case-sensitive in `map!`. Unfortunately, this may be more awkward to implement than [#3](#3). Depending on how awkward, we may as well be consistent with `strings!`s and `char!`s throughout the language and go all the way with [#2](#2), which is a much better solution IMO. However, this one is less radical.


#### 5
**Like `hash!` and `block!`, `map!` is case-sensitive for storage, but case-insensitive for lookup (by default).**

Then which is chosen for adding and modifying keys with a `:`? I would want to be able to use this convenient syntax for case-sensitivity. Great care needs to be taken here, and additional proposals would have to be made to ensure that the advantages of `map!` over `hash!` are not compromised. I see too many opportunities to do this badly for it to be worthwhile. It may not be impossible, but there would be more problems to solve and decisions to make. More has already been discussed about this than I care to touch on here. A separate proposal should probably be made if this were to be seriously considered.


#### 6
**There are 2 datatypes: a case-sensitive `strict-map!`, and a case-insensitive `relaxed-map!` (but with better names)**

If the reason for a `relaxed-map!` is solely consistency, then this has no advantage over [#3](#3), since the existence of a `strict-map!` breaks that consistency anyway. However, there may be other uses for `relaxed-map!` to be included.


#### 7
**`map!` has a case-sensitivity property.**

The first problem I see for this is that it may desimplify the literal syntax for `map!`. Which is the default? Is information lost for the other? Or are there 2 literal non-constructive notations? If so, then this is really just [#6](#6) again.


#### 8
**`map!` doesn't have `word!` keys, and all it's keys are case-sensitive.**

In this scenario, if you want `word!` keys, use `object!`s. I haven't yet thought through the consequences of this one much.


#### 9
**Case-sensitivity for the Red language can be set in the header.**

Pros:
* Could make Red a very interesting language.

Cons:
* A default and inheritance rules would need to be decided upon.
* Headers would need to be included with snippets of example code.
* Code snippets probably wouldn't be able to be safely copied/pasted from one file to another.
* Seems like overkill to get the best of both worlds, when the best is probably not what it will provide.


#### 10

Variant of [#2](#2), in order to avoid competing /relaxed/strict refinement explosions.  Case-awareness of words will still be necessary for dialects that want to extract that information.  So words will need to be able to be selected with `select/strict` *(suggested as better than /case)*.  Then strings--being biased the other way--will need `select/relaxed`.  It is already a nuisance to deal with the refinements as they are... because utility routines that wrap SELECT or FIND (or similar) need to propagate the `/case` refinement in their interface.

Alternative idea which may ease this: give strings the ability to carry a "case matters or not bit".  Default to "case matters", then have something like `~"foo bar"` and `~<div>` for creating uncased strings/tags/etc.  *(For such a notation, a prerequisite is Plan Minus Four or similar, such that `~"foo bar"` is not interpreted as `~ "foo bar"`.  One of the many benefits of expanded notational possibilities Plan -4 provides.  Also `~=` is much better for "approximately equal" as @rebolek points out.)*

Retain original casing in the data, and offer functions for twiddling the bit one way or the other...so that a string created as cased can be made uncased or vice-versa.  Letting the value carry the search property would probably take a load off of a lot of situations in terms of needing to pass through /STRICT.

    tag: ~<foo>
    assert [tag == <FoO>] ;-- "uncased" ANY-STRING! passes even STRICT-EQUAL?
    set-strict tag
    assert [tag != <FoO>] ;-- "cased" ANY-STRING! fails even relaxed DIFFERENT?

*(Note: for why @HostileFork proposes `different?` instead of `not-equal?`, contemplate why `unless` isn't called `if-not`, and shouldn't be.)*
Raises some questions about how to handle adding an uncased string to a map that already contains matching cased ones, or adding a cased version of a string if it has an uncased one.  Maps have sort of a "quiet override" at present:

    >> make map! ["a" 10 "a" 20]
    == make map! [
        "a" 20
    ]

Simple idea of a cased variant wiping out a pre-existing uncased variant or uncased wiping out all pre-existing cased variants isn't the worst thing that could be chosen, and is at least easy.


***** PLEASE INSERT OTHER UNIQUE IDEAS ABOVE HERE *****


### Votes

[#2](#2), [#1](#1), [#4](#4), [#3](#3) -WiseGenius

[#1](#1), [#2](#2), [#6](#6)           -Rebolek (also, `~` should be `~=` IMO)

[#2](#2) with `~=` -Johnk

[#10](#10) was inspired by [#2](#2), as an attempt to address what I consider to be the main defect in #2.  So that's my first and second choice, with no interest in the others.  I strongly oppose [#1](#1) and added notes in the "cons" section.  Canon caselessness (admittedly a complex problem) should be doubled-down upon and done correctly...vs "doing what everyone else does because it's easy".  Passing the buck would take away the important ["freedom from"](http://blog.hostilefork.com/freedom-to-and-freedom-from/) case-sensitivity that is a unique and liberating feature in a like-written-language system.  - @HostileFork

[#1](#1), [#2](#2), [#6](#6)           -gnat

[#2](#2) Another long read, Case sensitive when it makes sense. When Words are case-sensitive, all variables need to be declared and we are back programming in C, so opposed to [#1](#1) - iArnold

[#10](10), [#2](#2).  As @HF had said any of the variants of the "Outer Space" proposal opens up many doors syntax wise, and one of the benefits we can tap into is being able to specify metadata regarding the encoding of the string, or whether when passed around it is either treated as having case sensitivity or not.  But if people don't want to go down that road, I'd definitely support #2. - iceflow19

[#2](2), [#4](4). - ChrisRG

[#10](10), [#2](#2) - Adrian - I wonder if those who voted for #2 as first or second, before HF's refinement of that into #10, would restate their preference in light of this new option. 

***** PLEASE ADD YOUR VOTES ON A LINE ABOVE HERE, IN DESCENDING ORDER OF PREFERENCE, WITH NO GUARANTEE THEY WILL BE COUNTED *****