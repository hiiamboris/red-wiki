The parse function is case insensitive by default (but only for a-z at the moment) but does not handle bitset!s case insensitively. The current behaviour, mirrored in Rebol 2, could be considered inconsistent.

```red
>> parse "abc" [some [#"A" | #"B" | #"C"]]
== true
>> c: charset [#"A" #"B" #"C"]
== make bitset! #{
00000000000000000E0000000000000000000000000000000000000000000000
}
>> parse "abc" [some c]
== false
```

It is proposed that given Unicode and the design assumption of matching against the bit corresponding to the Unicode codepoint as a given, this matching should still be done with case-folding (effectively case-insensitive) or without case-folding (case-sensitive). In effect, this boils down to two or one checks against the bitset!. (Case sensitive when the /case refinement of parse is specified).

Some users have reported using the current behaviour to implement partial case sensitive matching. The proposed (CASE / NO-CASE parse enhancement) [http://www.rebol.net/wiki/Parse_Project#CASE_and_NO-CASE_keyword_pair] seems a far better design, than using charsets as a workaround for this purpose.