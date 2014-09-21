The case for this was made in CC#2108. It has the advantage of putting LOCAL: out of band (along with RETURN:, EXTERN:, WITH:) so that it can't be expected to be used as a refinement, e.g.

```
>> x: func [/local z] [if local [print z]] 
>> x/local "silly..."
silly...
(In addition to being silly it could trip up some security scenarios.)
```
Also, it does not make /LOCAL an illegal name as a refinement. Existing routines using /LOCAL instead of LOCAL: would have the consequence of allowing the above, as well as showing /LOCAL in the help.

For Red/System supporting both doesn't matter in the near term, as it still doesn't have refinements (right?) If it did, then presumably the same issue - using /LOCAL would mean you could invoke with the parameters, while local: would not allow it and be actual locals. Until refinements were implemented there could just be no difference.