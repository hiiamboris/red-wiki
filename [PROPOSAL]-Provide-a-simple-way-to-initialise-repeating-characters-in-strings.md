Provide a simple way to intialise strings such as:

	s: (20)"*"				;; string of 20 *s
	s: "^[20] "				;; string of 20 " "
	s: (50)""				;; an empty string with length of 50, size 51
	s: "^[50]"				;; ditto

-----

Would this functionality be better served by a function? (old idea, with name and exact behavior TBD) --Gregg
```red
dup: dupe: dupl: func [
	"Returns a value repeated 'count times in a block."
	value
	count [integer!]
	/str "Return a string, rather than a block, if /into is not used"
	/into "Put results in out, instead of creating a new block"
		out [series!] "Target for results, when /into is used"
][
	default out make either str [""] [[]] count
	case [
		series? :value [
			loop count [out: insert/only out copy/deep value]
		]
		any-function? :value [
			loop count [out: insert/only out value]
		]
		'else [insert/dup out value count]
	]
	either into [out][head out]
]
```