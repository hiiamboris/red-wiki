
See also: [Error Handling](https://github.com/red/red/wiki/%5BDOC%5D-Error-handling)

As of June-2021 Red's error messages aren't great yet. We know that. Red has not yet developed accurate error reporting because we haven't yet solved the source code to loaded code mapping problem. We want messages to be helpful but, before we can do that, we need accurate information. We are absolutely not satisfied with error information as it stands today. The work on it was suspended until we resolve the mapping problem or give up on it and fall back to the less accurate (but in some cases more helpful) `NEAR:` location info like Rebol.

One of the blocking point for the above text<=>code mapping is the limited `cell!` size. If we expand it for 64-bit support, we should have enough space to store the extra data to create that mapping. It's all tradeoffs, but this may be a good use of modern hardware and space, as programmer time and debugging effort are both largely unsolved problems. Red's design compounds this issue because, while it starts out as lines of text in files, once `load`ed that information is not maintained. Module design needs to take this into consideration as well, which can add value at a higher level. But it doesn't solve the problem for cases where macros are used, or code is generated dynamically. This is the mapping problem. How do we add metadata to loaded code to help with this?

Ideas and suggestions are welcome here, as well as links to other systems that do things well, along with a summary of why they're good. Bad examples are also welcome. 

Remember that we can't always just do what other languages or tools do, but we can learn from them.

