Red uses the sytnax shown in https://static.red-lang.org/red-system-specs.html#section-4.1 for hex integer literals.

The current hex format is a tradeoff, to be sure. We don't want to do it like other langs, as that's not a good fit for Red. Many alternatives have been considered, but no clear winner emerged. It's not likely to cause conflict, unless you call something `FACEh` or tombstone values as `DEADh`, but people will surely complain about case sensitivity inconsistency. They will be used much more in Red/System code.

It's a pragmatic tradeoff, but should be considered in cases where code generation or untrusted data are in use. Once we're in RedLand (not too close a neighbor to Redmond), type checking is your friend. That is, if you `load` a string containing these values, you have to be aware that some values that look like words, at a glance, will load as integers.

It's a nice, and very quiet syntax, but that subtlety has pros and cons. It's yet another design choice, for which there is no perfect solution. Just something to be aware of.

- https://github.com/rebol/rebol-issues/issues/2197

# TO-HEX

# Binary!

