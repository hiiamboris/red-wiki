This paper, [I wrote the fastest hashtable](https://probablydance.com/2017/02/26/i-wrote-the-fastest-hashtable/), is the start.

This extract from the Lua Mailing List is the second step:

>> 2017-02-27 19:12 GMT+08:00 Daurnimator quae@daurnimator.com:

>>I read this today:
https://probablydance.com/2017/02/26/i-wrote-the-fastest-hashtable/

>>I thought it might be interesting reading for many of you.

>>I'm eager to see how lua's algorithm compares, and what lessons might be learnt.

>I feel Lua's implement is much faster than this. it use a limited
linear search, but Lua's use a linked list into open address. with a
linked list, Lua's get a free node is much easier than author's.
when insert, Lua also move the colliding node into it's main position.
in my amoeba library[1], in a 1000+ elements hash table, the most link
has only 3 elements. and the lookup loop looks even more compact than
author's:
```c
static const am_Entry am_gettable(const am_Table t, am_Symbol key) {
const am_Entry *e;
if (t->size == 0 || key.id == 0) return NULL;
e = am_mainposition(t, key);
for (; e->key.id != key.id; e = am_index(e, e->next))
if (e->next == 0) return NULL;
return e;
}
```
>the most inspired thing is the limited linear search, in Lua it means
limited search in lastfree, it's not easy to add this change to my
hashtable, and I will do some research for this change. but in current
test, the worst case for max loop for lastfree is the whole list
length. so maybe it will have a big improve.

>it's really a very impressive thing, TIL :grinning:
--
regards,
Xavier Wang.

Third are Gregg's comments and embedded links:

The integer modulo bit was very interesting. I realized I was 1/3 of the way through and skimmed the rest lightly. It seems there will always be a point where you go from "really simple, and somewhat scalable" to "not terribly complex and scalable for most needs" to "so complex, it's not worth it unless you have the exact need it solves only at massive scale".
The security aspect and attack vectors were also interesting, which comes to having examples that avoid your weaknesses, because people will emulate them.
In the past, I often said Rebol would scale for most people's needs, even used naively and inefficiently. I do that myself at times, knowing there's a tradeoff. Now, though, it's tricky, because so much data is so big, while a person's individual data is still small (e.g. number of contacts and messages). If Red will be applied to data science, machine learning, or even small business with bigger data, we do have to think about scaling.

Gregg Irwin @greggirwin 05:14
http://lemire.me/blog/2016/06/27/a-fast-alternative-to-the-modulo-reduction/

Gregg Irwin @greggirwin 05:52
Man, lots of interesting stuff. Must get to other work now though.
http://lemire.me/blog/2015/10/26/crazily-fast-hashing-with-carry-less-multiplications/
https://github.com/lemire/StronglyUniversalStringHashing

