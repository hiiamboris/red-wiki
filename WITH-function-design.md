`With` is a great name, and many of us through the Rebol years have created similar versions for accessing values in a context, from a block. PR #1156 supports multiple contexts, but I think it's worth widening the scope of the design chat, as it may also be a good name for use with channels, structured concurrency, and series operations. We don't want to make it big, complex, or overloaded in a way that's confusing, but we should make the most of the best words.

References: 

- https://github.com/red/red/pull/1156