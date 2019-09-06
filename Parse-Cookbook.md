This page is for `parse` examples that show solutions and patterns to common problems.

If you've never used `parse`, or as a dialect reference, look at 
https://www.red-lang.org/2013/11/041-introducing-parse.html
or check the links provided below.

Examples should be kept relatively small, but may be specific (drop in and use) or generalized patterns that give people a starting point to work from, since the example may not match their data exactly.

1. ["Split a string at a delimiter-pattern"](#split-a-string-at-a-delimiter-pattern)
1. [Parse text and change keywords-pattern](Parse-text-and-change-keywords)
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

# Parse text and change keywords (#pattern)
Use ``change`` to transform text including keywords. 

The need derived from transferring one patent query syntax into a different one.

Simple Example: ``(text1+ 2W text2) OR (text3 3W text4+)`` --> ``(text1* SEQ2 text2) OR (text3 SEQ3 text4*)``
Changes apply to ``<n>W --> SEQ<N>``, ``+ --> *`` and so on.

```
text: {((edge S cloud) OR (edge W of W network+) OR (edge 2W server) OR ((iaas OR paas OR saas) S edge) 
OR (next_gen+ 2W cloud))/TI/AB/CLMS AND (G06F-009/5072 OR (H04L-067/10:H04L-067/1095) OR 
(H04L-067/12:H04L-067/125))/IPC/CPC}

digit:     charset "1234567890"
OPERATOR: ["/TI" | "/AB" | "/CLMS"]
rule: [any [ 
		change { W } { SEQ1 } 
		| change [copy match [digit "W"]] ( (match: copy/part match 1) rejoin ['SEQ match]) 
		| change { D } { NEAR1 } 
		| change [copy match [digit "D"]] ( (match: copy/part match 1) rejoin ['NEAR match]) 
		| change {+} {*}
		| change OPERATOR {}
		| change { S } { ???<S>??? } ; not supported in new syntax
		| change { F } { ???<F>??? } ; not supported in new syntax
		| skip]
	]
	parse text rule
print text
```
Output:

``((edge ???<S>??? cloud) OR (edge SEQ1 of SEQ1 network*) OR (edge SEQ2 server) OR 
((iaas OR paas OR saas) ???<S>??? edge) OR (next_gen* SEQ2 cloud)) AND 
(G06F-009/5072 OR (H04L-067/10:H04L-067/1095) OR (H04L-067/12:H04L-067/125))/IPC/CPC
``


# Links to pages about `parse`

* ["The reference for Red parse including examples like 
``IPv4 address validation``, ``email adress validator``, ``math expression validation``, ``HTML parser``, ``interpreter for obfuscated language``"](https://www.red-lang.org/2013/11/041-introducing-parse.html)

* ["A view links collected here on GitHub"](https://github.com/red/red/wiki/%5BDOC%5D-Parse#learning-resources-for-parse)

* ["A beginner friendly Parse article by Michael Sydenham"](http://www.michaelsydenham.com/reds-parse-dialect/)

* ["red-lang.org overview of Parse"](https://www.red-lang.org/2013/11/041-introducing-parse.html)

* ["Parse chapter from Ungaretti's Red Language Notebook"](https://ungaretti.gitbooks.io/red-language-notebook/content/parse.html)

* ["Parse section from Red by Example"](http://www.red-by-example.org/parse.html)
