# Thoughts on Why Immutability Matters

From @numberjay:

re: immutability.
Let me state: Red should not be immutable by default, or it wouldn't be Red anymore.

But providing immutable data structures would be lovely.
Failing that, it's not the end of the world.
Because Red is well suited to let me implement any data structure, immutable or not. 

You nail it down about threads, immutability is a huge win there and i also agree with you about not being fond of threads anyway, preferring approaches that are simpler to reason about. 

In my current Clojure project i'm building a structural IDE (where code is laid out as a tree and looks like a mind map) and when i had to implement the undo/redo functionality it took me a measly 2 hours... all thanks to immutability. 

Why?

To keep the state of your app, if all you have is mutable data structures... either you limit your undo/redo or it will take as much time to implement as all the other features.
You basically have to reimplement them in 'reverse' so to say.
We're talking about days, more likely weeks.
And any new feature you add, you have to again work on the undo/redo for taking it into account... 
No surprise undo/redo is one of the most boring implementation tasks you might deal with.

But i hold the state of my app in an immutable hashmap and by simply pushing/popping each hashmap (for each new state) to a couple vectors... undo/redo is as easy as traversing the vectors back and forth and emptying the redo vector if i generate a new state for my app.

With immutability i got undo/redo for free, literally.
Then i added more features and they already came with undo/redo (no need to do anything!).
Instead, using and copying a mutable hashmap each time would be totally unfeasable performance wise (both in terms of memory and time).

As undo/redo is a universal feature expected in any real world app, i would say that immutability matters.
And i just gave you a relevant real world example, that's not the only use case of course!

About the complexity of a copy-on-write tree-based approach, again i'm not wishing at all for Red to be immutable by default... 
I'm not implying or even hinting at Nenad to work on adding immutable data structures (if he did, it would be awesome of course).
Only... it's not as complex as it seems, it's a solved problem and even if i'm not inclined to plunge into the specific literature... i'm sure i could pull it off in a practically satisfactory way from scratch (even if maybe suboptimal, but then performance can always be optimized after a correct implementation is in place). 

In the end the concept is quite simple, mapping a linear structure onto a tree. 

What is more troubling instead, is not being able to use arbitrary data as keys in maps and hashes...
Someone here suggeted that hash! does allow blocks as keys, but Nenad himself clarified in the answer to my ticket #3084 that no, it doesn't.

What Nenad states is very logical and true in the general case, but disappointing for two reasons:

1) on a practical level, suggesting i use a block instead of an hash/map is a great no no if my hash/map is huge (it is) and my keys are tiny blocks of 3 or 4 elements (they are).
Hashing a small block is insanely faster than traversing a million elements block to find a key...
But again that's not the end of the world, i think i can pull it off anyway by molding my tiny blocks into strings and using *those* as keys... (hashing strings, but why?!? :-).

2) on a philosophical level, and this bothers me most, as a programmer i feel disempowered not by a real difficulty or impossibility (hashing any series is straightforward, a non issue), but by an opinion... that i would be using huge data structures as keys (i'm not gonna do that in general) and that would be slow (it is)... while i would like to decide by myself what price i'm willing to pay based on what i want to do.

I hope i didn't come out as confrontational because i'm not.

If i didn't care about Red and its goals and philosophy, i wouldn't consider switching from Clojure (i'm switching) and taking the time to express myself here.

