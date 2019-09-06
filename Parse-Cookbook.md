This page is for `parse` examples that show solutions and patterns to common problems.

If you've never used `parse`, or as a dialect reference, look at 
https://www.red-lang.org/2013/11/041-introducing-parse.html
or check the links provided below.

Examples should be kept relatively small, but may be specific (drop in and use) or generalized patterns that give people a starting point to work from, since the example may not match their data exactly.

1. [Split a string at a delimiter](split-a-string-at-a-delimiter)
1. [Reorder content](reorder-content)
1. []()


# Split a string at a delimiter (#pattern)

Look no further than the standard `split` func's current incarnation. It shows how to use `collect` and `copy` to extract data as it moves through the string looking for delimiters.

```
split: func [
	{Break a string series into pieces using the provided delimiters} 
	series [any-string!]
	dlm [string! char! bitset!]
	/local s num
][
	num: either string? dlm [length? dlm] [1] 
	parse series [
		collect any [
			copy s [to [dlm | end]] keep (s) num skip [end keep ("") | none]
		]
	]
]
```

# Reorder content (#pattern)


# Links to pages about `parse`

* ["The reference for Red parse"](https://www.red-lang.org/2013/11/041-introducing-parse.html)

* ["A view links collected here on GitHub"](https://github.com/red/red/wiki/%5BDOC%5D-Parse#learning-resources-for-parse)

* ["A beginner friendly Parse article by Michael Sydenham"](http://www.michaelsydenham.com/reds-parse-dialect/)

* ["red-lang.org overview of Parse"](https://www.red-lang.org/2013/11/041-introducing-parse.html)

* ["Parse chapter from Ungaretti's Red Language Notebook"](https://ungaretti.gitbooks.io/red-language-notebook/content/parse.html)

* ["Parse section from Red by Example"](http://www.red-by-example.org/parse.html)
