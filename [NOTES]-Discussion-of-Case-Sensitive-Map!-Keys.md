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
* In plaintext forums (or even preferred often in forums where markup is available), Rebol programmers have traditionally used all uppercase to signal an identifier.  So in this mixed-case sentence I could say GREATER-OR-EQUAL? and it is known that I am referring to something specific.  But if case-sensitivity were in the mix, it really would be possible that `GREATER-OR-EQUAL?` would be distinct from `greater-or-equal?` or `Greater-Or-Equal?` or `Greater-Or-EQUAL?` ad nauseum.  While the challenges of this particular loss aren't hard to address when markup is available (or by using the now-common backtick), the uppercase is cleaner and this points to ground lost.

Note:
* Most modern languages are like this now.
Scheme effectively went through this change in the revision from R<sup>5</sup>RS to R<sup>6</sup>RS. In this case, however, the community seemed to request it.
http://www.r6rs.org/final/html/r6rs-rationale/r6rs-rationale-Z-H-2.html#node_toc_node_sec_4.1.2
* May need to introduce something like `~=` from [#2](#2) below.
* `select`'s `/case` refinement would need to be dropped as useless, and something like `/relaxed` would need to be added, etc.
* What new word/s or refinement/s would have to be added to `parse`? Should such be already added anyway?
* So far, most who prefer this option seem to do so on the grounds of consistency and easier implementation.
A question to ask here, is “Is there anyone in the community who actually wants case-sensitive words because they miss them from other languages and want to use them?”
* So far, most who oppose this option seem to do so based on a fear of a greater opportunity to make typos. In my opinion, this is an irrational fear which is rarely complained about in practice by those using case-sensitive languages. Such typos would be most likely to occur when using camel-case, which from what I've seen, doesn't seem to be a Rebol convention. Most seem to type all in lowercase anyway.
* Personally, if Red were a brand new language and Rebol didn't exist, then full case-sensitivity would be my first vote. However, because I appreciate the history behind Red, [#2](#2) (and some of its varients) gets my first vote as seeming more appropriate.


#### 2
**Throughout the entire Red language, all `word!`s are case-insensitive but `string!`s and `char!`s are case-sensitive.**

This follows from the observation that:
* Most of the arguments for case-insensitivity seem to be for `word!`s.
* Most of the arguments for case-sensitivity seem to be for `string!`s.

One possible way to achieve this might be if `=` were strict for `string!`s and relaxed for `word!`s. Then `==` would be strict for both, and something like `~=` (previously `~`, see last note below) would be relaxed for both. Then, for example, `select` would use `=`, `select/strict` (replacing `select/case`) would use `==`, while something like `select/relaxed` could use `~=`.
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
* Something like `~=` might be useful for `char!`s anyway.
* The `map!` problem may simply be a symptom of a deeper problem with trying to stay consistent with a flawed system. I personally think this proposal is a more powerful system to stay consistent with.
Perhaps `hash!`, `block!` and others can take advantage of case-sensitive `string!`s.
* New words and refinements would probably need to be added to `parse`.
* Personal note: This is the behaviour I expected when I first used Rebol, initially thinking `=` had a bug until I discovered `==`.
* **`~=` rather than `~`:** When I first suggested [#2 in chat](http://chat.stackoverflow.com/transcript/message/21852929#21852929), I was going to suggest `~=` as the `relaxed-equal?` infix operator proposed here, but I wrote `~` because it was easier to type, it was available and I expected someone to come up with an entirely better idea or implementation. When rewriting it here above, I initially kept it as `~` because I expected it to have resistance and wanted to see what others would suggest as a better name. Without any prompting, many have independently pointed out that `~=` is a better choice. I tried to continue to leave it unchanged as `~` to keep historical context for the comments in the [votes](#votes), but then [#10](#10), [#11](#11) and [#12](#12) were added. Each additional idea is an extension of [#2](#2), each one uses `~=` for `relaxed-equal?` and each one additionally uses `~` for something else. The need for constant clarification when referring to [#2](#2) was becoming too unwieldy. Therefore, I've changed `~` to `~=` in the main suggestion. I've also added this note, hoping it substitutes for the context of the comments in the [votes](#votes).


Examples:

```red
	red>> "a" = "A"
	== false
	red>> "a" == "A"
	== false
	red>> "a" ~= "A"
	== true

	red>> 'a = 'A
	== true
	red>> 'a == 'A
	== false
	red>> 'a ~= 'A
	== true

	red>> select ["a" 10 "A" 20] "A"
	== 20
	red>> select/strict ["a" 10 "A" 20] "A"
	== 20
	red>> select/relaxed ["a" 10 "A" 20] "A"
	== 10

	red>> select [a 10 A 20] 'A
	== 10
	red>> select/strict [a 10 A 20] 'A
	== 20
	red>> select/relaxed [a 10 A 20] 'A
	== 10
```

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

Pros:
* Most compatable with Rebol. Even more compatable than Rebol 3's current `map!`.

Cons:
* Like `hash!` and `block!`, using `map!` for case-sensitive data misses out on the syntactic sugar of path syntax. However, this doesn't hurt utility, and a new proposal could always be made to add a case-sensitive option for path.
* Assuming that the case sensitivity of a particular set of data would remain the same throughout its lifetime, having to remember which syntax to use when accessing which set of data **might** be an opportunity for human error. This is some of the reasoning behind [#6](#6), [#7](#7), [#11](#11), [#12](#12) and [#13](#13).

Note:
* The comments which were originally written here have been removed since they were a mistake, and were intended for an accidentally skipped idea which has now been added as [#14](#14).

#### 6
**There are 2 datatypes: a case-sensitive `strict-map!`, and a case-insensitive `relaxed-map!` (but with better names)**

If the reason for a `relaxed-map!` is solely consistency, then this has no advantage over [#3](#3), since the existence of a `strict-map!` breaks that consistency anyway. However, there may be other uses for `relaxed-map!` to be included.


#### 7
**`map!` has a case-sensitivity property.**

The first problem I see for this is that it may desimplify the literal syntax for `map!`. Which is the default? Is information lost for the other? Or are there 2 literal non-constructive notations? If so, then how different is this from [#6](#6)?.


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
* Cannot switch between one way or the other within the same file. All `map!`s, etc. must be one way within a file.
* Seems like overkill to get the best of both worlds, when that is probably not what it will provide. Rather than best of both in one language, this effectively splits both worlds into 2 languages.


#### 10

**Like [#2](#2), but give values a "strictness bit" (true by default for strings, false by default for words).  Allow the bit to be twiddled programmatically, or expressed by alternate literal forms and construction syntaxes.  Comparisons will be done as non-strict if *either* value being compared does not have the strictness bit set...even if /STRICT refinements, ==, !==, etc are used.**

The motivation behind this proposal is to avoid bringing /relaxed into the picture in the matrix of comparison or find operations.  Having STRICT variants of things already explodes [a non-trivial family of functionality](https://github.com/hostilefork/rebol-proposals/blob/e27964e835f617ecbd9a68b1e416219b6e8400b2/comparison-operators.reb#L266).  This may also be able to push back against a current nuisance for anyone trying to wrap up an operation like SELECT or FIND with having to pass the /STRICT refinement through, by letting the values themselves get a vote.

Literal forms of strings which override the default non-strict-compare might look something like `~"foo bar"` and `~<div>` for creating uncased strings/tags/etc.  *(For such a notation, a prerequisite is Plan Minus Four or similar, such that `~"foo bar"` is not interpreted as `~ "foo bar"`.  One of the many benefits of expanded notational possibilities Plan -4 provides.)*  This would line up nicely with `~=` for a non-strict comparison.

    tag: ~<foo>
    assert [tag == <FoO>] ; "uncased" ANY-STRING! passes even STRICT-EQUAL?
    set-strict tag
    assert [tag != <FoO>] ; "cased" ANY-STRING! fails "relaxed" DIFFERENT?

*(Note: for why @HostileFork proposes a name like `different?` instead of `not-equal?`, contemplate why `unless` isn't called `if-not`.)*

This concept raises some questions about how to handle adding an uncased string to a map that already contains matching cased ones, or adding a cased version of a string if it has an uncased one.  Maps have sort of a "quiet override" at present:

```red
    >> make map! ["a" 10 "a" 20]
    == make map! [
        "a" 20
    ]
```

Simple idea of a cased variant wiping out a pre-existing uncased variant or uncased wiping out all pre-existing cased variants isn't the worst thing that could be chosen, and is at least easy.

The floor is open for ideas on what a strict notation for WORD! might be.  A construction syntax could look like `word![= "StrictCased"]`

#### 11
##### 11a
**Like [#2](#2), but extended so that `map!`, hash!`, block!`, and others have a bit whose non-default setting is to allow all operations such as `select` to automatically have a `/relaxed` refinement.**

In the examples below, we are assuming a possible notation in which `~[...]` represents a block with the bit switched to the non-default setting.

Now, besides all the examples in [#2](#2), we also have:

```red
red>> select ~["a" 10 "A" 20] "A"
== 10
```

The question remains whether the `/strict` refinement will override the bias of the collection:

```red
red>> select/strict ~["a" 10 "A" 20] "A"
== 20
```

Depending on how it's implemented, this may be the same question as has been asked about mixing refinements in [#2](#2):

```red
red>> select/relaxed/strict ["a" 10 "A" 20] "A"
== 20
```
If `/strict` **does** override a relaxed bias (as in both the examples above):
* A `map!` with the bit switched to the non-default setting will effectively behave like `map!` from Rebol 3, except that:
	* An initial `make map! ~["a" 10 "A" 20]` should not lose any information, and
	* A `/strict` refinement can still be used to work with that information.
* This gives `/strict` a little more utility.
* We might be tempted to think about adding yet **another** refinement to override it back to the default, rather than all the way to strict, and that's getting a bit rediculous!

If `/strict` **doesn't** override a relaxed bias:
* A `map!` with the bit switched to the non-default setting will effectively behave like `map!` from Rebol 3, without exception.
* This could be a big gotcha!
* If we have no interest in **ever** overriding (even default bias to `/strict`), we should seriously consider going all the way with [#12b](#12b).

Pros:
* Anticipating that `map!`s, `hash!`s, `block!`s, etc. are likely to only be used one way or the other throughout their lifetimes. This makes use convenient without having to keep specifying a `/relaxed` refinement for the same collection.

Cons:
* Makes `map!`s, `hash!`s, `block!`s, etc. a “bit” bigger.

Note:
* This extension of [#2](#2) is different from [#10](#10) in that there is a bit on `block!`, `hash!`, `map!`, etc. rather than on `string!`.
* This is similar to [#7](#7), but more sophisticated, since it is based on [#2](#2). Also, unlike [#7](#7), it also affects other datatypes such as `hash!`s and `block!`s.


##### 11b
**[#11a](#11a) without a `/relaxed` refinement.**

If we assume that all `map!`s, `hash!`s, `block!`s, etc. will be only be used one way or the other throughout their lifetimes, we may no longer need a `/relaxed` refinement, since:

```red
red>> equal?  select/relaxed ["a" 10 "A" 20] "A"  select ~["a" 10 "A" 20] "A"
== true
```
...and since this is redundant:

```red
red>> select/relaxed ~["a" 10 "A" 20] "A"
== 10
```

Pros (in addition to [#11a](#11a)):
* No need to worry about what happens when `/relaxed` and `/strict` refinements are mixed together.

Cons (in addition to [#11a](#11a)):
* Cannot use `/relaxed` spontaneously on a collection set to default, without making a non-default copy of it. *(I personally can't imagine any scenarios requiring this, though.)*


#### 12
##### 12a
**Like [#11a](#11a), but extended so that rather than use a simple bit, such datatypes could also be biassed to use a `/strict` refinement.**

For those wanting the option for `word!`s to be treated as case-sensitive in `map!`s, `hash!`s, `block!`s, etc. Maybe using something like `=[...]` to contruct a strict-biassed `block!`, for example.

Pros (in addition to [#11a](#11a)):
* Allows a `map!` to optionally hold multiple `word!` keys distinguished only by case without the cons of [#9](#9). *(This is the back-breaking straw which allowed [#12](#12) to overtake [#11](#11) as my new top vote.)*
* Might be useful for case-sensitive dialects?

Cons:
* Makes `map!`s, `hash!`s, `block!`s, etc. more than a “bit” bigger.

Notes:
* One way this could work internally is if `map!`s, `hash!`s, `block!`s, etc. have two "strictness bits": the first one for `string!`s, and the second one for `word!`s. In this order, `[..]` would have "strictness bits" `[true false]` (and would use `=`), `~[...]` would have bits `[false false]` (and would use `~=`), something like `=[...]` would be `[true true]` (and would use `==`), and the bit combination `[false true]` couldn't exist.
* This implies that `object!`s should also have a bit to allow them to have case-sensitive `word!`s. However, `object!`s would only need a single bit, because relaxed-biased `object!`s would behave like ordinary `object!`s since they only have `word!`s as keys anyway. Perhaps the relaxed syntax could optionally be used to construct an `object!`, but would return an ordinary `object!` with no difference. Likewise, a `/relaxed` refinement could be used on an `object!`, but would be redundant.

##### 12b
**[#12a](#12a) without `/strict` (or `/case`) or `/relaxed` refinements.**

If we assume that all `map!`s, `hash!`s, `block!`s, etc. will be only be used one way or the other throughout their lifetimes, we may no longer need `/strict` (or `/case`) or `/relaxed` refinements.

Pros (in addition to [#12a](#12a)):
* No need to worry about what happens when `/relaxed` and `/strict` refinements are mixed together.
* No need to worry about whether the refinement overrides the set bias of the collection.

Cons (in addition to [#12a](#12a)):
* Cannot use `/relaxed` or `/strict` (or `/case`) refinements spontaneously on a collection, without making a differently biased copy of it. *(I personally can't imagine scenarios requiring this, though.)*


#### 13
##### 13a
**`map!`, hash!`, block!`, and others have a case-sensitivity bit whose default setting is to be case-insensitive.**

Notes:
* This is like [#11a](#11a) and [#12a](#12a), except that it isn't based on [#2](#2), but rather on the way Rebol works now.
* This is similar to [#7](#7), except that it also affects other datatypes such as `hash!`s and `block!`s.
* If unsure about (or not ready for) the radical changes of [#2](#2) and it's derivatives, this seems like an easy way to get some of the advantages of [#11](#11) and [#12](#12) without such a radical change, and without breaking much if deciding to switch to one of them later. Maybe even build up to this one too by implementing [#6](#6) and then [#7](#7) first. This would give `map!` full utility in the meantime.
* If this were the final destination, and if `char!`s were also case-insensitive by default, there would be no need for `~=`.


##### 13b
**[#13a](#13a) without `/strict` (or `/case`) or `/relaxed` refinements.**

Pros and cons: Same ones as [#12b](#12b), but without those of [#2](#2).


#### 14
**There is no `map!`. `hash!` and `block!` have an attribute to set their default skip value. When attempting to change the value of a non-existent key, it is appended.**

Cons (in addition to [#5](#5)):
* This type of "map" has properties and functions exceeding its intended use, which could be a source for bugs.
* Great care needs to be taken here, and additional proposals would have to be made to ensure that the advantages of `map!` over `hash!` are not compromised. I see too many opportunities to do this badly for it to be worthwhile. It may not be impossible, but there would be more problems to solve and decisions to make. More has already been discussed about this than I care to touch on here. A separate proposal should probably be made if this were to be seriously considered.


***** PLEASE INSERT OTHER UNIQUE IDEAS ABOVE HERE *****


### Votes

[#12](#12), [#11](#11), [#2](#2), [#1](#1), [#13](#13), [#7](#7), [#5](#5), [#10](#10), [#4](#4), [#6](#6), [#3](#3) -WiseGenius

[#1](#1), [#2](#2), [#6](#6)           -Rebolek (also, `~` should be `~=` IMO)

[#2](#2) with `~=` -Johnk

[#10](#10) was inspired by [#2](#2), as an attempt to address what I consider to be the main defect in #2.  It's somewhat off-the-cuff, and needs thinking through.  But it's a direction for making #2 acceptable when I find a `/relaxed` unacceptable--so let's call that my first and second choice for now, with no interest in the others.  I strongly oppose [#1](#1) and added notes in the "cons" section.  Canon caselessness (admittedly a complex problem) should be doubled-down upon and done correctly...vs "doing what everyone else does because it's easy".  Passing the buck would take away the important ["freedom from"](http://blog.hostilefork.com/freedom-to-and-freedom-from/) case-sensitivity that is a unique and liberating feature in a like-written-language system.  - @HostileFork

[#10](#10), [#1](#1), [#9](#9), [#2](#2), [#6](#6)    I am hoping red and rebolX will become much more introspective and malleable with regard to internals. For example, I might want to replace find with a FIND that acts more intelligently.  In general, choice is better than no choice. --gnat

[#2](#2) Another long read, Case sensitive when it makes sense. When Words are case-sensitive, all variables need to be declared and we are back programming in C, so opposed to [#1](#1) - iArnold

[#10](#10), [#2](#2).  As @HF had said any of the variants of the "Outer Space" proposal opens up many doors syntax wise, and one of the benefits we can tap into is being able to specify metadata regarding the encoding of the string, or whether when passed around it is either treated as having case sensitivity or not.  But if people don't want to go down that road, I'd definitely support #2. - iceflow19

[#2](#2), [#4](#4). - ChrisRG

[#10](#10), [#2](#2) - Adrian - I wonder if those who voted for #2 as first or second, before HF's refinement of that into #10, would restate their preference in light of this new option. 

[#1](#1)           -jacobgood1

[#1](#1), [#2](#2) - Andreas (NB: If [#2](#2) is chosen, that still leaves the door open for [#10](#10) or other improvements on the /relaxed /strict situation necessitated by pure #2. As it stands, I think #10 still needs a lot of thorough thinking and design; too much so, for me to be able to endorse it as a preferred solution as it stands.)

[#2](#2), Arie. Alternatively consider adding a different datatype for case sensitive csstring! / csmap! Under the hood it would work the same I guess.

[#1](#1) and if not [#2](#2) - Peter Wood - Case insensitivity may be great in an insular environment but becomes a barrier in a more open environment. 

[#1](#1), [#2](#2) - Oldes

[#5](#5), [#2](#2) - Gregg

***** PLEASE ADD YOUR VOTES ON A LINE ABOVE HERE, IN DESCENDING ORDER OF PREFERENCE, WITH NO GUARANTEE THEY WILL BE COUNTED *****