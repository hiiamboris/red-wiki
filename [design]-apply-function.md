APPLY is proving to be a challenge to design. There are too many ways to do it, some of which compete for a particular interface model. Most importantly, we have to consider how easy it will be to use correctly, as well as how flexible it is for power users.

An important aspect is that we need to compare apples to apples. If we add an example for one model, we have to include versions of it for all other models for comparison. The examples may into different use case scenarios.

We've been all over the map on this, and need to consolidate to avoid losing our minds further.

# Use cases

See: https://github.com/greggirwin/red-hof/blob/master/apply.md#use-cases

We should have examples for each.

# Models

This is where we document and name each interface option.


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
	; c.1)
	apply 'append/:only [output value block? value]
	; c.2)
	apply/safer 'append/:only [output value (block? value)]
	; NR§2.1	This doesn't make fixed refinements distinct
	apply :append [series value /only]
	; BB4.2) BM§op-ref-2)
	apply 'append [series value /only block? value]
]

; Variant where /dup is also applied used if arg is a block.
; a.1
emit: func [value][
	; dupe values 3 times if they are blocks
	only: dup: block? value 
	; if NOT a block, count is not consumed, so we have to be aware
	; and not just make it the last expression in the func.
	res: append/:only/:dup output value 3
	res
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
	;?? Is it correct that fixed args are not pass in the block?
	; c.1
	apply 'append/dup/:only [series value block? value count] ; Are both of these legal?
	apply 'append/dup/:only [series value count block? value]
	; c.2
	apply/safer 'append/dup/:only [series value (block? value) count] ; Are both of these legal?
	apply/safer 'append/dup/:only [series value count (block? value)]
	; NR§2.1	This doesn't make fixed refinements distinct
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

# Comments and voting

Lots of subjectivity here, so we need a way to cast votes as data, not prose and commentary.
