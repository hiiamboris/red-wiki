Red is a multi-paradigm language. If you want, you can write using a single model, or you can mix and match to make your intent clear, and leverage what each approach has to offer. Solving certain problems lend themselves to a particular paradigm, but there may be tradeoffs. For example, recursion can be wonderful but Red doesn't yet have TCO (Tail Call Optimization), so recursion currently has a cost in Red that another functional language avoids. You also need to consider your target reader. If you're a team of OO or imperative programmers, there is no shame in writing code that you all understand, rather than using a functional approach. 

Red also has ways of doing things that are unique to it, because of its design and core functionality. For example, it has a lot of datatypes, far more than most languages. And it's homoiconic (it is its own data format), so Red data is very easy to process. Add the `parse` function to that, and the ability to create dialects (a.k.a. DSLs) easily, and you have more paradigms.

This page is a place where we can collect examples of how a problem might be solved using different approaches. Bear in mind that the goal is not just to have the shortest solution, but to have the clearest, correct solution that readers can understand. Sometimes we're maintaining code, and sometimes learning from others. Some approaches make it easier to get things right the first time, or to test, or to make fence-post errors stand out (or even eliminate them). Some are more performant, others more robust or flexible. That is to say, we're not looking for a "winner" here. There is no single best solution for every person, team, or context.

When adding new items for comparison, copy the template section and fill it in. If you have a new paradigm you want to include, add it to your own section, but not the general template. We could add other standard paradigms (dataflow, logic, array oriented, etc.), but that will come later, if ever. 

*NOTE: All solutions MUST be implemented in Red. This is not a comparison with other languages, but with what different approaches look like in Red.*

[Programming Paradigms](https://en.wikipedia.org/wiki/Programming_paradigm)

[Symbolic](https://en.wikipedia.org/wiki/Symbolic_programming) programming is interesting in Red, and lines can be blurry. Just as functional programming doesn't *require* the use of recursion, symbolic programming in Red may extend beyond the current program, by creating and consuming dialected data.

----------------

# Template

## Imperative

## Functional

## Object Based or Object Oriented

## Reddish

*(These could be idiomatic solutions, parse-based, dialects, etc., and should have some explanation to go with them, for readers not deeply familiar with Red. That is, say why this might be a good approach, or even why it isn't.)*

----------------

# find-Nth

Find the Nth value in a series.

## Imperative

```red
find-Nth_Imp: function [
	"Returns the series at occurrence N of value, or none."
	series [series!]
	value
	n      [integer!]
][
	while [n > 0][
		n: n - 1
		pos: find/only series value
		either pos [series: next pos][break]
	]
	pos
]
```

## Functional

```red
find-Nth_Rec: function [
	"Returns the series at occurrence N of value, or none."
	series [series!]
	value
	n      [integer!]
][
	; TBD require positive n value
	;print ['find-Nth_Rec mold series value n]
	pos: find/only series value
	either n = 1 [pos][
		if pos [find-Nth next pos value n - 1]
	]
]
```

## Object Based or Object Oriented

## Reddish

This is a clear and simple approach, but doesn't handle all cases correctly. e.g., if `value` is a lit-word or block.

```red
; This works under R2, but not Red
find-Nth_Par-0: func [
	"Returns the series at occurrence N of value or none."
	series [series!]
	value
	n      [integer!]
	/local count pos
][
	count: 0
	parse series [
		some [
			to value pos: (
			   count: count + 1
			   if count = n [return pos]
			) skip
		]
	]
	none
]

find-Nth_Par-1: function [
	"Returns the series at occurrence N of value, or none."
	series [series!]
	value
	n      [integer!]
][
	n: n - 1
	parse series [n [thru value] to value pos:]
	pos
]
```

## Examples

```red
find-Nth "abcabcabc" #"a" 3
find-Nth "abcabcabc" "bc" 2
find-Nth [a b c a b c a b c] quote 'a 3    ; note that `quote` is needed here for find-Nth_Par-1
find-Nth [a b c [a] b c a b c] [a] 1       ; invalid for find-Nth_Par-1
find-Nth [a b c #a b c a b c] #a 1
```

----------------
