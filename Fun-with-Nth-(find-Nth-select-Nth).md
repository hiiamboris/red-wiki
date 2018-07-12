How do you `find` or `select` the Nth occurrence of a value in a series? There are many ways to approach it. Let's compare some; their clarity, performance, limitations, and quirks should be considered.

Code dump from @greggirwin to prime the pump (others should pull theirs from July-2018 Gitter chat):
```
; Recursive approach
find-Nth_Rec: function [
	"Returns the series at occurrence N of value, or none."
	series [series!]
	value
	n      [integer!]
][
	; TBD require positive n value
	;print ['find-Nth mold series value n]
	pos: find/only series value
	either n = 1 [pos][
		if pos [find-Nth next pos value n - 1]
	]
]
;print mold find-Nth "abcabcabc" #"a" 3
;print mold find-Nth "abcabcabc" "bc" 2
;print mold find-Nth [a b c a b c a b c] 'a 3
;print mold find-Nth [a b c [a] b c a b c] [a] 3
;print mold find-Nth [a b c #a b c a b c] #a 3

; Imperative approach
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
;print mold find-Nth "abcabcabc" #"a" 3
;print mold find-Nth "abcabcabc" "bc" 2
;print mold find-Nth [a b c a b c a b c] 'a 3
;print mold find-Nth [a b c [a] b c a b c] [a] 3
;print mold find-Nth [a b c #a b c a b c] #a 3

;find-Nth_Imp-1: function [
;	"Returns the series at occurrence N of value, or none."
;	series [series!]
;	value
;	n      [integer!]
;][
;	data: #()
;	forall series [
;		v: first series
;		if none? data/:v [data/:v: copy []]
;		append data/:v series
;	]
;	pick data/:value n
;]
;print mold find-Nth_Imp-1 "abcabcabc" #"a" 3
;print mold find-Nth_Imp-1 "abcabcabc" "bc" 2
;print mold find-Nth_Imp-1 [a b c a b c a b c] 'a 3
;print mold find-Nth_Imp-1 [a b c [a] b c a b c] [a] 3
;print mold find-Nth_Imp-1 [a b c #a b c a b c] #a 3

; Parse based
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

; @hiiamboris
select-nth: func [s v n] [
	all [parse s compose/only [(n) thru quote (:v) s: to end] s/1]
]
; until GC is there:
select-nth: func [s v n] [
	all [parse s compose/only/into [(n) thru quote (:v) s: to end] clear [] s/1]
]
; GC-independent, and also a bit faster:
select-nth: func [s v n] [
	parse s head change/only skip [n thru quote v s: (return s/1)] 3 :v
]

find-Nth_Par-2: function [
	"Returns the series at occurrence N of value, or none."
	series [series!]
	value
	n      [integer!]
][
	n: n - 1
	parse series either any-block? :value [
		[n [thru into value] to into value pos:]
	][
		[n [thru value] to value pos:]
	]
	pos
]
find-Nth: :find-Nth_Par-2
print mold find-Nth "abcabcabc" #"a" 3
print mold find-Nth "abcabcabc" "bc" 2
print mold find-Nth [a b c a b c a b c] first ['a] 3	; just 'a doesn't work
print mold find-Nth [a b c a b c a b c] quote 'a 3
print mold find-Nth [a b c [a] b c a b c] ['a] 1
print mold find-Nth [a b c #a b c a b c] #a 1
```

