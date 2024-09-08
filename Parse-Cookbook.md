This page is for `parse` examples that show solutions and patterns to common problems.

If you've never used `parse`, or as a dialect reference, look at 
https://www.red-lang.org/2013/11/041-introducing-parse.html
or check the [links](#Links-to-pages-about-parse) provided below.

Examples should be kept relatively small, but may be specific (drop in and use) or generalized patterns that give people a starting point to work from, since the example may not match their data exactly.

1. [Split a string at a delimiter-pattern](#split-a-string-at-a-delimiter-pattern)
2. [Parse text and change keywords-pattern](#Parse-text-and-change-keywords-pattern)
3. [Parse text where term is at end of line](#Parse-text-where-term-is-at-end-of-line)
4. [Parse text with paired brackets](#Parse-text-with-paired-brackets)
5. [Keep, keep pick and keep copy](#Keep-keep-pick-and-keep-copy)

# Split a string at a delimiter (#drop-in)

Look no further than the standard `split` func's current incarnation. It shows how to use `collect` and `copy` to extract data as it moves through the string looking for delimiters.

```Red
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
***
# Parse text and change keywords (#pattern)
Use ``change`` to transform text including keywords. 

The need derived from transferring one patent query syntax into a different one.

Simple Example: ``(text1+ 2W text2) OR (text3 3W text4+)`` --> ``(text1* SEQ2 text2) OR (text3 SEQ3 text4*)``
Changes apply to ``<n>W --> SEQ<N>``, ``+ --> *`` and so on.

```Red
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

```Red
((edge ???<S>??? cloud) OR (edge SEQ1 of SEQ1 network*) OR (edge SEQ2 server) OR 
((iaas OR paas OR saas) ???<S>??? edge) OR (next_gen* SEQ2 cloud)) AND 
(G06F-009/5072 OR (H04L-067/10:H04L-067/1095) OR (H04L-067/12:H04L-067/125))/IPC/CPC
```

## Alt example

Convert `MARKER <num>KEY` sections to `MARKER NEW-KEY<num>`.
```
digit=: charset "0123456789"

mod-str: func [input /local num= =num mark][
    input: copy input
    num=: [copy =num some digit=]
    parse  input [
        any [
            ["rfid " num= change #"W" "SEQ" insert =num]
            | skip
        ]
    ]
    input
]

s1: "(rfid 2W radio)"
s2: "(rfid 10W frequency)"
s3: "(rfid 999W frequency)"
s4: "(rfid wwW frequency)"
s5: "(rfid 99 frequency)"

print mod-str s1
print mod-str s2
print mod-str s3
print mod-str s4
print mod-str s5
```

***
# Parse text where term is at end of line
The need derived from detecting a term only at the end of a line.
Simple Example: detect ``OR`` in ``{abc def OR}`` respectively ``{abc def OR }`` with a trailing space.
The answer is simple using the keyword ``end`` i.e.:  _return success if current input position is at end_.
Usually examples go with a match phrase ``to end``.

```Red
OR-rule: [{OR} opt space end (print "{OR} detected")]
```

```Red
>> parse {abc orc def OR } [some [OR-rule | {or} (print "stumbled on {or}") | skip]]
stumbled on {or}
{OR} detected
== true
>> parse {abc orc def} [some [OR-rule | {or} (print "stumbled on {or}") | skip]]
stumbled on {or}
== true
>> parse {abc def ORC } [some [OR-rule | {or} (print "stumbled on {or}") | skip]]
stumbled on {or}
== true
```
***
# Parse text with paired brackets
Different approaches to parse strings of the following outlook:<br>
`(abc OR def)/TI`<br>
So there are outer brackets and a /TI at the end. 
We do not look inside the brackets, as long as there is a /TI at the end.
It becomes tricky with the following strings:<br>
`(abc OR (def AND GHI) )/TI` or <br>
`( ( abc AND JKL) OR (def AND GHI))/TI` or <br>
`( ( ((abc AND JKL) OR (def AND GHI) )) )/TI`<br>

The result shall be a logic return `true`or `false`.<br>
There shall be no "hidden" /TI inside the string.<br>
The brackets shall be balanced.

In gitter.im/red/help the following working solutions were provided.
Each approach has it's beauty.<br>
### Count-based approach (by Toomas)
```Red
[(pars: 0 yep: no) any [
   #"(" (pars: pars + 1) 
   | ")/TI" end (yep: true) 
   | #")" (pars: pars - 1) 
   | skip
   ] if (all [pars = 1 yep])
]
```
### paren!-based approach (by Vladimir)
```Red
source: {
    ((((abc AND JKL) OR (def AND GHI))))/TI
}
probe parse load source [paren! /TI]
```

`load` here is used to convert `string!` to a `block!` of values; see [this](https://github.com/red/red/wiki/%5BNOTES%5D-How-Red-Works---A-brief-explanation) page for a brief explanation.

The use of `paren!` is instructive, i.e. just a check whether a series of items is enclosed in parantheses.

### Object-based approach (by Vladimir)
```Red
source: trim/lines {
    (abc OR def)/TI
    (abc OR (def AND GHI))/TI
    ((abc AND JKL) OR (def AND GHI))/TI
    ((((abc AND JKL) OR (def AND GHI))))/TI
}

grammar: object [
    counter: 0
    bump: func [delta][counter: counter + delta]
    balanced?: does [zero? counter]
    rule: [
        some [
            some [
                  #"(" (bump +1) 
                | #")" (bump -1)
                | not "/TI" skip
            ] 
            "/TI" if (balanced?)
        ]
    ] 
]
probe parse/case source grammar/rule
```
***
# Keep, keep pick and keep copy

As explained [here](https://github.com/red/red/wiki/%5BDOC%5D-Guru-Meditations#parse-collectkeep-options-combined-with-tothru-rules), 
`keep` by itself collects a single value or a list of values in range-capturing mode.

This may yield a block of heterogeneous values, like `char!` mixed with `string!` in the following example:
```
>> parse "a,bc," [collect some [keep to "," skip]]
== [#"a" "bc"]
```
To get a series of homogeneous values, use:

* `keep pick` to collect all the matched values separately - eg.:
```
>> parse "a,bc," [collect some [keep pick to "," skip]]
== [#"a" #"b" #"c"]
```
    
* `keep copy <word>` to collect all the matched values as a series (of same type as the input) - eg.:
```
>> parse "a,bc," [collect some [keep copy _ to "," skip]]
== ["a" "bc"]
```
(`_` above is just a throwaway `word!`, could be x or z)

# Collect all set words

```
data: [
    foo: 33
    bar: 66
	id: 123
	lots: [
		lot: [name: "blackberry"]
		lot: [name: "apple"]
		lot: [
			name: "bananas"
                        number: 43
			obj: [ price: 44 ]
			obj: [ price: 44 ]
			]
	]
]		
```

collect all set words:

```		
 parse data rule: [
     some [
            set w set-word! (probe w) | not block! skip
         |  ahead block! into rule     
     ]
 ]

```

collect all set-words that values are `block!`:
```
parse data rule: [
    some [
           ahead [set-word! block!] set w set-word! (probe w) | not block! skip
        |  ahead block! into rule     
    ]
]
```

See this [discussion](https://rebol.tech/gitter.im/red/help/2020/#msg5f0c99923e4a827d19c35da8) for more on that subject.

***
# Links to pages about `parse`

* ["The reference for Red parse including examples like 
``IPv4 address validation``, ``email adress validator``, ``math expression validation``, ``HTML parser``, ``interpreter for obfuscated language``"](https://www.red-lang.org/2013/11/041-introducing-parse.html)

* ["A view links collected here on GitHub"](https://github.com/red/red/wiki/%5BDOC%5D-Parse#learning-resources-for-parse)

* ["A beginner friendly Parse article by Michael Sydenham"](http://www.michaelsydenham.com/reds-parse-dialect/)

* ["Parse chapter from Ungaretti's Red Language Notebook"](https://ungaretti.gitbooks.io/red-language-notebook/content/parse.html)

* ["Parse section from Red by Example"](http://www.red-by-example.org/parse.html)

* ["Rebol Core Users Guide Chapter 15 - Parsing"](http://www.rebol.com/docs/core23/rebolcore-15.html)

* ["Parse tutorial for Rebol 2 found on codeconscious.com"](http://www.codeconscious.com/rebol/parse-tutorial.html)

* ["Introduction to Red Parse on hostilefork.com"](http://blog.hostilefork.com/why-rebol-red-parse-cool/)

* ["Wikibook page on Rebol Parse including examples like ``remove-chars``"](https://en.wikibooks.org/wiki/Rebol_Programming/Language_Features/Parse/Parse_expressions)

* ["stackoverflow Blog on parse and translate DSL using Red with Red code"](https://stackoverflow.com/questions/48454538/how-to-parse-and-translate-dsl-using-red-or-rebol)

* ["Helpin' Red -> Parse by Ungaretti"](http://helpin.red/Parse.html)
