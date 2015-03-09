Currently, the default handling of values of the word!, path! and string! types is on a case insensitive basis within Red. This value consistency allows consistent function behaviour.

Applying this consistency to the map! datatype implies that values of the word!, path! and string! datatypes would be case insensitive.

There is a case that map! keys, especially string! values, should be case sensitive.

***** PLEASE INSERT THE CASE FOR CASE SENSITIVE MAP! KEYS HERE *****
A case sensitive key makes it possible to use a base-64 coded string as key.
***** PLEASE INSERT THE CASE FOR CASE SENSITIVE MAP! KEYS HERE *****

There are a number of options to provide case sensitive map! keys:

**1. Throughout the language, all word!s, string!s and char!s are case-sensitive.**
This gives us full consistency and utility. This would be my first preference after #2. However it's quite radical and, as earl points out, the majority of rebolers would apparently not welcome this change.

**2. Throughout the language, all words!s are case-insensitive and string!s and char!s are case-sensitive.**
I see this as still consistent (since this is actually the behaviour I expected when I first used Rebol, thinking = had a bug until I discovered ==), however depending on how this is implemented, it could be perceived as inconsistent.
One possible way to achieve this might be if = were strict for string!s and relaxed for word!s. Then == would be strict for both, and something like ~ would be relaxed for both. Then, for example, select would use =, select/case (or select/strict) would use == while something like select/relaxed could use ~.
This could be seen as either more or less radical than #1. I personally think it's more in line with what Rebol is than #1.

**3. All keys in map! are the case-sensitive exception.**
This is probably the easiest to implement in the short term, and the easiest exception to teach if breaking consistency.

**4. map!'s string! and char! keys are the case-sensitive exception.**
This is a compromise to make word!s consistent, but not string!s and char!s. I would prefer this to #3. Unfortunately, it may be more awkward to implement. Depending on how awkward, we may as well be consistent with strings!s and char!s and go all the way with #2 which is a much better solution IMO. However, this is less radical.

**5. Like hash! and block!, map! is case-sensitive for storage, but case-insensitive for lookup (by default).**
Then which is chosen for adding and modifying keys with a :? I would want to be able to use this convenient syntax for case-sensitivity. Great care needs to be taken here, and additional proposals would have to be made to ensure that the advantages of map! over hash! are not compromised. I see too many opportunities to do this badly for it to be worthwhile. It may not be impossible, but there would be more problems to solve and decisions to make.

**6. Throughout the language (including map! keys), all word!s, string!s and char!s are case-insensitive.** This is not an acceptable solution since it poses the problem to be solved in the first place.

**7. There are 2 datatypes: a case-sensitive strict-map!, and a case-insensitive relaxed-map! (names not important)**
I think this is silly! The reason for a relaxed-map! is consistency, but the existence of a strict-map! breaks that consistency anyway. This has no advantage over #3, IMO.

**8. map! has a case-sensitivity property.**
The first problem I see for this is that it may de-simplify the literal syntax for map!. Which is the default? Is information lost for the other? Or are there 2 literal non-constructive notations? If so, then this is really just #7 again.

**9. map! doesn't have word! keys, and all it's keys are case-sensitive.**
In this scenario, if you want word! keys, use object!s. I haven't thought through the consequences of this one much.