The charset function does not automatically generate "case insensitive" bit sets other than specify both upper case and lower case letters.

Parse itself is case insensitive by default (but only for a-z) so the current behaviour could appear confusing to some users:

```
>> parse "abc" [some [#"A" | #"B" | #"C"]]
== true
>> c: charset [#"A" #"B" #"C"]
== make bitset! #{
00000000000000000E0000000000000000000000000000000000000000000000
}
>> parse "abc" [some c]
== false
```

A refinement to charset that creates the appropriate bitset! with both the upper and lower case bits would perhaps reduce the confusion. (This would allow a Unicode module to replace a basic charset function with a Unicode aware one too.)

Changing the default behaviour would probably remove the confusion for the new user but would cause inconvenience for long time users.