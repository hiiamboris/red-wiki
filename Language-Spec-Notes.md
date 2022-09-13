# Float! comparisons

```
a: 0.1 + 0.1 + 0.1   b: 0.3
; than:
equal? a b ;== true
; but:
lesser-or-equal? a b ;== false
```

Gregg said:

It's easy to explain, but not necessarily intuitive. We also have to weigh completeness and practical use. That's where the design falls now, though it isn't documented. By this I mean that comparisons for equality are likely used in different ways than their lesser/greater counterparts. Here it makes sense to nearness for the common case, while allowing a strict alternative, as doing it the other way around would be awkward. It does mean there are gaps in consistency however. @meijeru may have thoughts here.

My gut instinct is that the `lesser/greater+-or-equal?` should be relaxed, as this makes things consistent. It does mean a different gap in logic, and we have to ask if that needs to be filled. We don't have to do it until someone complains, and that can also be worked around for those rare cases. While still awkward, those cases should be much rarer.