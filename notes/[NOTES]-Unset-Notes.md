- http://www.rebol.net/r3blogs/0318.html
- https://github.com/red/red/issues/3112
- https://github.com/red/red/issues/3404

## Notes for future doc wording and explanations

It's something that people don't need to know about right away. Some may never even understand it fully. It's the same situation as binding/scope and datatype actions. There is a user experience side whose job it is to hide some (many) of these details. Or, as in the case of function specs, to hide the details of how they work behind a mental image that lets people use them without understanding how they really work.

There's no "solving it" as long as we have unset!, and I don't see that going away. It is useful, in addition to easing some implementation details (while making others more complicated). What we can do is discuss Special Values in more detail. In most user docs we can use "no value" terminology, lightly note that internally it's called unset!, and say "If you encounter this, from exit, or by evaluating an empty block or paren, you can uset set/any and get/any to avoid errors. But you shouldn't use them intentionally. Don't try to hold on to them and pass them around. Just be aware they exist. Oh, and they're especially handy when printing to the console, because they leave the output clean, void of an == ... result, where you just want to show data."

## There is no unset in Topaz

@giesse
> @hiiamboris you can handle it in R2 too. Ladislav wrote a number of examples. The fact that we had to work hard to handle the general case was what made me and him talk to Carl about it and propose different solutions (in person in the 2004 DevCon).

@hiiamboris
> :+1:

@giesse
> It is. But there are compatibility concerns as well. Me and Ladislav wanted to get rid of `unset!` too for eg. but that didn't pass. :) One of the reasons I made Topaz was to actually test those ideas (among many others).


@hiiamboris
> @giesse did you get rid of unset in Topaz?

@giesse
> yes


@hiiamboris
> Or you just hid it? E.g. you have lit-word `'no-value` - what value does it have internally?

@giesse
> @hiiamboris well, in the current implementation internally it would be Javascript's `null`. You can think of it as a hidden `unset!` if you wish, though it is never really the result of anything and it's not an actual Topaz value.
> 
> ```
> >> i-have-no-value
> *** Script error: Word has no value: i-have-no-value
> *** Stack: i-have-no-value
> >> context-of 'i-have-no-value
> == context [
> ==     conjure: native [
> ==         "Conjure a value"
> ==         name [word!]
> ==       ...
> >> get/any 'i-have-no-value
> == none
> ```


@hiiamboris
> OK. And if I want to suppress console output, how do I go without unset?


@giesse
> @hiiamboris I don't see why, but, if you're hard pressed for a special case, you could use `none` for that.


@hiiamboris
> Just curious. I don't like unset either.

> OK just checked:
> ```
> Topaz Interpreter - (C) 2011 Gabriele Santilli - MIT License
> >> do []
> == none
> >> ()
> == none
> ```


@giesse
> ```
> >> get make word! "no-context"
> *** Script error: Word has no context: no-context
> *** Stack: get make word! "no-context"
> ```

## Reduce and Compose

It might seem like these two funcs would treat unset results the same. Both evaluate, but `compose` evaluates only parens. `Compose` is special in this regard, by design. While `reduce` leaves unset values in place, `compose` makes them disappear.

```
>> type? first probe reduce [()]
[unset]
== unset!
>> type? first probe compose [()]
[]
== none!
>> type? first probe compose/only [()]
[]
== none!
```

