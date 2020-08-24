# Learning resources for Parse.

* ["Official Parse documentation"](https://github.com/red/docs/blob/master/en/parse.adoc)

* ["red-lang.org overview of Parse"](https://www.red-lang.org/2013/11/041-introducing-parse.html)

* ["Parse chapter from Ungaretti's Red Language Notebook"](https://ungaretti.gitbooks.io/red-language-notebook/content/parse.html)

* ["Parse section from Red by Example"](http://www.red-by-example.org/parse.html)

***
# Parse Cookbook
https://github.com/red/red/wiki/Parse-Cookbook
***
# Notes about Parse

[Parse always reaches the tail with some combinations of iterators and TO / THRU](https://github.com/red/red/issues/3679)

## `To/thru` with block rules

`parse "aabbccdd" [ thru [p: (probe p) "cc" ]]` produces this output:
```
"aabbccdd"
"abbccdd"
"bbccdd"
"bccdd"
"ccdd"
```

This is because `parse` allows block rules as arguments to `to/thru`, which exposes the inner search loop to the user. That is, you get to peek inside the workings and see how `to/thru` proceed and finally match. Without the `probe` you wouldn't see that, of course.