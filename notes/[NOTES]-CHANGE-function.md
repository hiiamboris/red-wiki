*(Just pulling some chat notes to be cleaned up later)

I notice that an issue has been resolved by the following change:
```red
-                pos: change pos value
+                pos: insert remove pos value
```
But change and insert remove have the same effect ?!? Or am I missing something?

Semseddin Moldibi @endo64 07:05
The problem is changeing with empty string doesn't advance series! position, same for R2 & Red, see the below code:
```red
>> head change find "test" "e" ""
== "test"
So, using it in a while loop causes an infinite loop
>> change find "test" "e" ""
== "est"
```

There is also another point using change that we need to be careful about:

```red
>> head change find "test" "e" "xy"
== "txyt"  ; s also replaced 
>> head insert remove find "test" "e" "xy"
== "txyst" ; s is not replaced
```

Rudolf Meijer @meijeru 07:12
I conclude that change needs much more explanation.

Semseddin Moldibi @endo64 07:35
You are right, same happens for block!s as well, if /only is not used:
```red
>> head change find ["t" "e" "s" "t"] "e" ["x" "y"]
== ["t" "x" "y" "t"]
>> head change/only find ["t" "e" "s" "t"] "e" ["x" "y"]
== ["t" ["x" "y"] "s" "t"]
```

Toomas Vooglaid @toomasv 07:36
@endo64 Good examples! Are these then equivalent:
```red
>> head change/part find "test" "e" "xy" 1
== "txyst"
>> head insert remove find "test" "e" "xy"
== "txyst"
And conversely:
>> head change find "test" "e" "xy"
== "txyt"
>> head insert remove/part find "test" "e" 2 "xy"
== "txyt"
```

Semseddin Moldibi @endo64 07:42
I think so. I never used/think about change/part with 1. It also works a bit faster:
```red
>> profile/show/count [_change_ _insert_] 1000000
Count: 1000000
Time         | Time (Per)   | Memory      | Code
0:00:01.832  | 0:00:00      | 43913216    | _change_
0:00:02.094  | 0:00:00      | 46014464    | _insert_
```

Toomas Vooglaid @toomasv 07:43
Good to know!

Toomas Vooglaid @toomasv 08:02
Also to note:
```red
>> head insert remove find "test" "est" "o"
== "tost"
>> head change find "test" "est" "o"
== "tost"
>> head insert remove/part find "test" "est" 3 "o"
== "to"
>> head change/part find "test" "est" "o" 3
== "to"
```

`remove` removes by default just on item, or number of items determined by /part.

`change` changes by default the length of replacement value, or the number of items determined by /part.

Toomas Vooglaid @toomasv 08:09
But it might be best to use replace instead:
```red
>> replace "test" "est" "o"
== "to"
>> replace "test" "e" "xy"
== "txyst"
```