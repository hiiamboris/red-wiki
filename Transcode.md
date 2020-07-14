1. [Check balanced parens](#check-balanced-parens)


# Check balanced parens

Credits: (@toomasv)

```
Sometimes you can also use the new transcode to check your strings for balanced parens:
>> transcode/trace %{a(b(c)d)e}% func [e i t l o][[open close] print [e mold i]]
open "(b(c)d)e"
open "(c)d)e"
close ")d)e"
close ")e"
== [a (b (c) d) e]
>> transcode/trace %{a(b((c)d)e}% func [e i t l o][[open close] print [e mold i]]
open "(b((c)d)e"
open "((c)d)e"
open "(c)d)e"
close ")d)e"
close ")e"
*** Syntax Error: (line 1) missing ) at (b((c)d)e
*** Where: transcode
*** Stack:  

>> transcode/trace %{a(b(c)d))e}% func [e i t l o][[open close] print [e mold i]]
open "(b(c)d))e"
open "(c)d))e"
close ")d))e"
close "))e"
close ")e"
*** Syntax Error: (line 1) missing ( at )e
*** Where: transcode
*** Stack:
```