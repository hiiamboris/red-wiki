Red uses the sytnax shown in https://static.red-lang.org/red-system-specs.html#section-4.1 for hex integer literals.

The current hex format is a tradeoff, to be sure. We don't want to do it like other langs, as that's not a good fit for Red. Many alternatives have been considered, but no clear winner emerged. It's not likely to cause conflict, unless you call something `FACEh` or tombstone values as `DEADh`, but people will surely complain about case sensitivity inconsistency. They will be used much more in Red/System code.

It's a pragmatic tradeoff, but should be considered in cases where code generation or untrusted data are in use. Once we're in RedLand (not too close a neighbor to Redmond), type checking is your friend. That is, if you `load` a string containing these values, you have to be aware that some values that look like words, at a glance, will load as integers.

It's a nice, and very quiet syntax, but that subtlety has pros and cons. It's yet another design choice, for which there is no perfect solution. Just something to be aware of.

- https://github.com/rebol/rebol-issues/issues/2197

# TO-HEX

# Binary!

# Syntax is temporary

> The current hex literal format has always been considered temporary, to cover short-term needs. IIRC, we have proposed a final literal hex format somewhere in one ticket, after many discussions (using 0#prefix).

There's a ticket with the format discussion: https://github.com/red/red/issues/1079

And a trello card on implementing it: https://trello.com/c/mI0MnhvD/151-find-a-better-literal-form-for-hexadecimals-see-1079

Source: [June 23, 2018 6:56 PM](https://rebol.tech/gitter.im/red/red/2018/#msg5b2e6d917da8cd7c8c679bae)