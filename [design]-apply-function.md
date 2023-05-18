APPLY is proving to be a challenge to design. There are too many ways to do it, some of which compete for a particular interface model. Most importantly, we have to consider how easy it will be to use correctly, as well as how flexible it is for power users.

An important aspect is that we need to compare apples to apples. If we add an example for one model, we have to include versions of it for all other models for comparison. The examples may fall into different use case scenarios.

We've been all over the map on this, and need to consolidate to avoid losing our minds further.

# Use cases

See: https://github.com/greggirwin/red-hof/blob/master/apply.md#use-cases

We should have examples for each.

Dynamic arguments are the norm, of course but refinements are not, and they may also specify args (params) to go with them. That's the pain point in Red, without `apply` because you end up dispatching with `either` or `case` and using hard-coded paths for the refinements.

## Extend

Function extension

## Trace

Tracing calls

```
sys-find: :find
my-find: func spec-of :sys-find [
	; dump args, do extra stuff.
	; call sys-find
]
```

## Propagate

Passing around a set of refinements+arguments. Simliar to Extend.

```
dup: function [
	{Returns a new block with the fill value duplicated n times.} 
	value
	count [integer!] "Negative numbers are treated as zero" 
	/only
	/string "Return a string instead of a block"
][
	series: copy either string [""] [[]]
	append/dup/:only series value count
]

repend': func [
	{Appends a reduced value to a series and returns the series head} 
	series [series!] 
	value 
	/only "Insert block types as single values (overrides /part)."
	/dup "Duplicate the inserted values."
		count [integer!]
][
	value: reduce :value
	res: append/:dup/:only series value count
]
```

## Generate

Programmatic call construction. Call a func with dynamic arguments/refinements
known from some data. 


## Other Thoughts

Should we also consider indirection of dynamic elements. e.g., rather than
writing the paths or propagating refinements from the spec, could you do this
(as a mezz):

```
apply-ex: func [fn [word!] refs [path! block!] args [block!]][
	; Do some AOP tricky stuff
	fn: to path! head insert copy refs fn
	res: apply fn args
	; AOP post processing
]
apply-ex 'append [/dup/only] [series value count]
```

This only adds a different syntax compared to NR§2.1/c*.

Does this fall under /tracing?


# Models

This is where we document and name each interface option. e.g.

## Dynamic refinements (straight-sugar, dynamic direct, dynamic context, dyna-path) (a?)

### Design

Get-word refinements are evaluated. If true, eventual optional arguments will be fetched.

`foo/:ref1/:ref2/ref3/... <arg1> <arg2> ...`

### Notes

Looks like a regular func call, but refinements can be get-words (like in a regular path) and are evaluated rather than being treated as literal/fixed/truthy. Their values come from the environment/context. Supported internally by semi-sweet.

```
path! <args> ; free ranging
append/dup/:only series value count ; /dup is fixed, /only is dynamic
<syntax>
```

## raw (stack frame block) (b?)

All args are required, in correct slots by spec order, but trailing false/none can be omitted.

```
apply function! block!
apply :foo [<arg1> <arg2> ...]
<syntax>
```

## semi-sweet (dynamic indirect, dynamic fixed arity, dyna-path) (a.2(old) or c.1(new)?)

Fixed arity version of straight sugar. Refinements go in the path. Their values come from the environment/context. Args, required or optional, go in the block.

```
apply [lit-word! lit-path!] block!
apply 'append/dup/:only [series value count]
<syntax>
```
Can we call the first two both dyna-path, because they are effectively the same except for the second being fixed arity?

Or maybe we don't name these at all, because they are the default.


## semi-processed (dynamic indirect no context, no context, path args)  (c.1?)

Fixed arity. Refinements go in the path. Values (on/off) for dynamic refinements go in block. Args, required or optional, go in the block.

```
apply [lit-word! lit-path!] block!
apply 'append/dup/:only [series value count true]
<syntax>
```

## processed (partial no context, no context, partial path args, dialected) (c.2?)

Fixed arity.  Word, not path. Everything goes in the block.

Requires `/some` refinement.

```
apply/some [lit-word! lit-path!] block!
apply/some 'append [series value /dup true count block? value]
<syntax>
```


As we can't seem to build consensus on any set of models, perhaps we can start with which models do have consensus (e.g. raw and straight-sugar). From there we decide what use cases those *don't* fit, and look for the next best model that fills those gaps. We also need to prioritize the case cases.


# Examples

This is where we list examples, noting their use case category, and what each model looks like for it. That means actual code. With luck we can keep it dense, because each one doesn't have to be run separately. It's about reading and comparing the alternatives.

From this, we should be able to see what models apply best for each use case, as well as determining if a model is redundant and can be done better using another model.

Each model implemented in the native R/S code of `apply` has to offer a benefit. The goal being to keep the R/S code small and test mezzanine wrappers for other models to see if their performance is acceptable.

## EX-1

Doc's canonical example. Decides internally to use a refinement.

NOTE: Old model names are in comments until new model names are defined.

```
output: copy []
emit: func [value][

	; original
	either block? value [append output value][append/only output value]

	; a.1)
	only: block? value
	append/:only output value

	; a.2)
	only: block? value
	apply 'append/:only [output value]

	; b) NR§1)
	apply/all 'append [output value false none block? value]

;	; c.1) (OLD)
;	apply 'append/:only [output value block? value]
;	; c.2) (OLD)
;	apply/safer 'append/:only [output value (block? value)]

	; c.1) (NEW)
	apply 'append/:only [output value block? value]
	apply/safer 'append/:only [output value (block? value)]
	; c.2) (NEW)
	apply/some :append [output value /only block? value]
	apply/some/safer :append [output value /only (block? value)]

	; NR§2.1	This doesn't make fixed refinements distinct
	only: block? value
	apply :append [output value /only]

	; BB4.2) BM§op-ref-2)
	apply 'append [output value /only block? value]
]

; Variant where /dup is also applied used if arg is a block.
; a.1
emit: func [value][
	; dupe values 3 times if they are blocks
	only: dup: block? value 
	; if NOT a block, count *IS*  consumed, which differs from a normal func call.
	append/:only/:dup output value 3
	; If the behavior worked like a regular func, we'd have to do this.
	;res: append/:only/:dup output value 3
	;res
]

; c.2
emit: func [value][
	; dupe values 3 times if they are blocks
	apply/safe 'append/:only/:dup [output value (block? value) (block? value) 3]
]
```

## EX-2

Uses a local value for a required arg, an apply-unrelated refinement, a fixed refinement, and a propagated dynamic refinement.

NOTE: Old model names are in comments until new model names are defined.

```
dup: function [
	{Returns a new block with the fill value duplicated n times.} 
	value
	count [integer!] "Negative numbers are treated as zero" 
	/only
	/string "Return a string instead of a block"
][
	series: copy either string [""] [[]]	; applies to all versions

	; a.1
	append/dup/:only series value count

	; a.2
	apply 'append/dup/:only [series value count]

	; b NR§1
	apply/all 'append [series value false none only true count]
	;?? Are refinement args enforced as `logic!`, Or is literal `/dup` evaluated as truthy, and still OK?

	; The following c* question being, if a refinement is fixed, not dynamic, does its arg(s)
	; have to fall in the same order as refinements in the path.

;	; c.1 (OLD)
;	apply 'append/dup/:only [series value block? value count] ; Are both of these legal?
;	apply 'append/dup/:only [series value count block? value]
;	; c.2 (OLD)
;	apply/safer 'append/dup/:only [series value (block? value) count] ; Are both of these legal?
;	apply/safer 'append/dup/:only [series value count (block? value)]

	; c.1) (NEW)
	apply 'append/:only/dup [series value block? value count]  ; fixed /dup not in arg block?
	apply/safer 'append/:only/dup [series value (block? value) count]
	; c.2) (NEW)
	apply/some :append [series value /only block? value /dup true count]
	apply/some/safer :append [series value /only (block? value) /dup true count]

	; NR§2.1	This doesn't make fixed refinements distinct
	dup: true
	apply :append [series value /dup count /only]
	apply :append [series value /only /dup count]

	; BB4.2) BM§op-ref-2)
	apply 'append [series value /only only /dup true count]

]
```

## EX-3 

Propagate all args and refinements

NOTE: Old model names are in comments until new model names are defined.

```
repend': func [
	{Appends a reduced value to a series and returns the series head} 
	series [series!] 
	value 
	/only "Insert block types as single values (overrides /part)."
	/dup "Duplicate the inserted values."
		count [integer!]
][
	value: reduce :value

	; a.1
	res: append/:dup/:only series value count

	; a.2
	apply 'append/:dup/:only [series value count]

	; b NR§1
	apply/all 'append [series value false none only dup count]

	; c.1
	apply 'append/:dup/:only [series value dup count only] ; Are both of these legal?
	apply 'append/:dup/:only [series value only dup count]
	apply 'append/:dup/:only [series value dup only count] ; No way to catch reversed logics, right?

	; c.2
	apply/safer 'append/dup/:only [series value (block? value) count] ; Are both of these legal?
	apply/safer 'append/dup/:only [series value count (block? value)]

	; NR§2.1	This doesn't make fixed refinements distinct
	apply :append [series value /dup count /only]
	apply :append [series value /only /dup count]

	; BB4.2) BM§op-ref-2)
	apply 'append [series value /only only /dup dup count]

]
```

## EX-4

More refinements here, many of which propagate, and one of which propagates through two different apply calls. No refinement args.

```
alter': function [
	"Alternate a value (append/remove), returning the toggled state (on/off)"
	input [series! bitset!] "If value is found, remove it; otherwise, append it (modified)"
	value     "Must be an integer, char, or string if input is a bitset"
	/only     "Treat block and typeset values as single values."
	/case     "Perform a case-sensitive match; ignored for bitset inputs."
	/same     "Use same? as comparator."
	/last     "Find the last occurrence of value, from the tail."
	/reverse  "Find the last occurrence of value, from the current index."
	/match    "Match at current index only."
][
	; a.1
	pos: find/:only/:case/:same/:last/:reverse/:match input value
	  ; body body body body
		append/:only input value
	  ; body body body body

	; a.2
	pos: apply 'find/:only/:case/:same/:last/:reverse/:match [input value]
	  ; body body body body
		apply 'append/:only [input value]
	  ; body body body body

	;!! NOTE: get-word args in the following examples can be plain words.
	
	; b NR§1
	; series value /part length /only /case /same /any /with wild /skip size /last /reverse /tail /match]
	pos: apply/all :find [input value false none :only :case :same false false none false none :last :reverse false :match]
	;pos: apply/all :find [input value false none only case same false false none false none last reverse false match]
	; Nobody wants to write this, or read it, but generated calls using this
	; model will look like this when debugging.
	;pos: apply/all :find [input value false none true true true false false none false none true true false true]
	  ; body body body body
		; series value /part length /only /dup count
		apply/all :append [input value false none :only]
		;apply/all :append [input value false none only]
	  ; body body body body

	; c.1
	; series value /part length /only /case /same /any /with wild /skip size /last /reverse /tail /match]
	apply 'find/:only/:case/:same/:last/:reverse/:match [input value :only :case :same :last :reverse :match]
	  ; body body body body
		; series value /part length /only /dup count
		apply/all :append/:only [input value :only]
	  ; body body body body

	; c.2 (same as c.1 since all args are single values)
	; series value /part length /only /case /same /any /with wild /skip size /last /reverse /tail /match]
	apply/safer 'find/:only/:case/:same/:last/:reverse/:match [input value :only :case :same :last :reverse :match]
	  ; body body body body
		; series value /part length /only /dup count
		apply/safer :append/:only [input value :only]
	  ; body body body body

	; NR§2.1	This doesn't make fixed refinements distinct
	apply :find [input value /only /case /same /last /reverse /match]
	  ; body body body body
		apply :append [input value /only]
	  ; body body body body

	; BB4.2) BM§op-ref-2)
	apply 'find [input value /only :only /case :case /same :same /last :last /reverse :reverse /match :match]
	  ; body body body body
		; series value /part length /only /dup count
		apply 'append [input value /only :only]
	  ; body body body body
	  
	; context carryover (#5)
	series: input					;-- can also be renamed in the function spec?
	apply :find 'local
		; for append a.1) is preferable
		
]
toggle: :alter'
```

## EX-5

Dynamic refinement simple case.

`capture-face` definition: https://codeberg.org/hiiamboris/red-view-test-system/src/commit/571f9d8e0725120e29d3c5afac07c0c40c53ece1/visuals.red#L91	

unordered key/value pairs with word shortcuts (original mezz impl):
```
	img: apply capture-face [
		face:  layout
		real:  real
		with:  not real  img: img
		whole: whole
	]
or
	img: apply capture-face [
		real whole
		face: layout
		with: not real  img
	]
```
a.1)
```
	with: not real							;-- blocks usage of `with` function in the calling function's body
	with': :system/words/with				;-- workaround
	img: capture-face/:real/:with/:whole layout img
```
BB4.2) BM§op-ref-2)
```
	img: apply capture-face [
		layout
		/real  real
		/with  not real  img
		/whole whole
	]
```

## EX-6

Dynamic refinement trickier case.

`send-request` definition: https://gitlab.com/rebolek/castr/-/blob/master/client-tools.red#L361

unordered key/value pairs with word shortcuts (original mezz impl):	
```
	response: apply send-request [
		method
		link:    url
		data:    post
		content: data
		with:    yes  args: compose/only [...]
	]
```
a.1)
```
	data2: data					;-- preserve data from being overridden
	data:  post
	send-request/:data/with method url data2 compose/only [...]
	data:  data2				;-- restore data for further usage
```
BB4.2) BM§op-ref-2)
```
	response: apply send-request [
		method url
		/data post data
		/with yes  compose/only [...]
	]
```

## EX-7

Function is unknown, defined in the object by the user, and may or may not support used refinements, and declares them in any order.

original code:
```
	spec:    spec-of :draw
	on?:     find spec /on
	window?: find spec /window
	draw: case [
		all [on? window?] [draw/window/on xy1 xy2 canvas fill-x fill-y]
		window?           [draw/window    xy1 xy2                     ]
		on?               [draw/on                canvas fill-x fill-y]
		'else             [draw                                       ]
	]
```
using current (slow) mezz `apply` implementation:
```
	apply draw [window xy1 xy2 on canvas fill-x fill-y]
```
a.1) b)
```
	not applicable here - code will be the same as the original
```
BB4.2) BM§op-ref-2) - only works if this mode supports /quiet refinement
```
	apply/quiet draw [
		/window window xy1 xy2
		/on     on canvas fill-x fill-y
	]
```
context carryover (#5) - as a replacement for /quiet
```
	;-- defined outside somewhere - to limit the set of carried arguments to only those known by the caller
	proxy-draw: func [draw /window xy1 xy2 /on canvas fill-x fill-y] [apply draw 'draw]
	;-- actual call:
	proxy-draw/window/on :draw xy1 xy2 canvas fill-x fill-y
```



# Comments and voting

Lots of subjectivity here, so we need a way to cast votes as data, not prose and commentary.
