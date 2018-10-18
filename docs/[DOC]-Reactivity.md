Primary reactivity docs are at https://doc.red-lang.org/en/reactivity.html

Notes on single triggering: https://github.com/red/red/issues/3096

PR for `get-word!` support: https://github.com/red/red/pull/3148

# Making reactors

When making reactors, you have control over how reactors made from the same spec behave. That depends on whether you `copy/deep` the spec, or not. If you have read https://github.com/red/red/wiki/Why-do-I-have-to-copy-series-values%3F, you know that Red is a data format first. If you have the same spec, but it appears multiple times as literal data, each is unique. If you use a word to refer to the *same* spec block, that same data is used. This is important in the reactivity subsystem. 

In this first example, we make 3 reactors from 3 different spec blocks. The blocks look the same, but each is a separate value. As you can see, each reactor is independent of the others, and likely the behavior you expect.

```red
>> a: make reactor! [x: 1 y: 2 total: is [x + y]]
== make object! [
    x: 1
    y: 2
    total: 3
]
>> a/x: 100
== 100
>> a/total
== 102
>> 
>> b: make reactor! [x: 1 y: 2 total: is [x + y]]
== make object! [
    x: 1
    y: 2
    total: 3
]
>> b/x: 200
== 200
>> b/total
== 202
>> a/x: 111
== 111
>> a/total
== 113
>> 
>> c: make reactor! [x: 1 y: 2 total: is [x + y]]
== make object! [
    x: 1
    y: 2
    total: 3
]
>> c/x: 300
== 300
>> c/total
== 302
>> b/x: 222
== 222
>> b/total
== 224
>> a/total
== 113
```

But what happens if the *same* spec is used to make multiple reactors. Here we set `spec` to reference the spec block, and re-use that for a second reactor (`b`). Everything looks normal, until we set `b/x:` and then examine `a/total`. Because the same spec block was used, the reactive subsystem connected it to the last reactor that was made from it. You can see that changes to `a/x` no longer trigger reactions in `a/total`, but changes to `b/x` are reflected in `a/total`. This is likely *not* the expected behavior. But Red can't read your mind. It *might* be what you want, so you can do it. And the way to produce the behavior from the first example is to use `copy/deep` on shared specs you use to make reactors. The `/deep` refinement is important here, because it's the `is` sub-block that you really need to copy. When we do this for the third reactor below (`c`), you can see that `b` does not suffer the same fate as `a` did.

```red
>> spec: [x: 1 y: 2 total: is [x + y]]
== [x: 1 y: 2 total: is [x + y]]

>> a: make reactor! spec
== make object! [
    x: 1
    y: 2
    total: 3
]
>> a/x: 100
== 100
>> a/total
== 102
>> 
>> b: make reactor! spec
== make object! [
    x: 1
    y: 2
    total: 3
]
>> b/x: 200
== 200
>> b/total
== 202
>> a/x: 111
== 111
>> a/total
== 202
>> 
>> c: make reactor! copy/deep spec
== make object! [
    x: 1
    y: 2
    total: 3
]
>> c/x: 300
== 300
>> c/total
== 302
>> b/x: 222
== 222
>> b/total
== 224
```

# Locals in reactive blocks

> How can I define and use local variables in reactive code, like in an `is` block?

You can use an anonymous function and invoke that.

```red
>> r: make reactor! [x: 5 y: is [do reduce [ has [z][z: 100 z + x + 5] ]]]
== make object! [
    x: 5
    y: func [/local z][z: 100 z + x + 5]
]
>> r/y
== 110
>> r/x: 0
== 0
>> r/y
== 105
```

# Issue with panels as reactive sources

@toomasv found this issue.
```red
; With `box`, `react` works while moving either colored box
view [
   size 500x300 
   at 0x0 base react [
      face/offset: n/offset 
      face/size: p/offset + p/size - n/offset
   ] 
   n: box loose red 50x50 
   p: box blue loose 
]
```
```red
; With `panel`, `react` works only when moving red box, not while moving blue panel
view [
   size 500x300 
   at 0x0 base react [
      face/offset: n/offset 
      face/size: p/offset + p/size - n/offset
   ] 
   n: box loose red 50x50 
   p: panel 100x100 blue loose []
]
```
With no pane spec for the panel, it works.
```red
view [
   size 500x300 
   at 0x0 base react [
      face/offset: n/offset 
      face/size: p/offset + p/size - n/offset
   ] 
   n: box loose red 50x50 
   p: panel 100x100 blue loose
]
```
You can work around it by adding the reaction after setting up the layout.
```red
lay: layout [
   size 500x300 
   n: box loose red 50x50 
   p: panel 100x100 blue loose []
]
insert lay/pane layout/only [
    at 0x0 base react [
        face/offset: n/offset 
        face/size: p/offset + p/size - n/offset
    ]
]
view lay
```
Or by putting the panel before the reaction is declared, then moving it to the top of the z-order.
```red
move-to-top: func [face] [move find face/parent/pane face tail face/parent/pane]

view/no-wait [
   size 500x300 
   at 60x60
   p: panel 100x100 blue loose []
   at 0x0 base react [
      face/offset: n/offset 
      face/size: p/offset + p/size - n/offset
   ] 
   n: box loose red 50x50 
]
move-to-top p
do-events
```
The cleanest way seems to be styling:
```red
view [
	size 500x300
	style node: box loose
	style pan: panel loose []
	at 0x0 base react [
		face/offset: n/offset 
		face/size: p/offset + p/size - n/offset
	] 
	n: node red 50x50 
	at 60x60 p: pan 100x100 blue
]
```
# Composing reactions in window `on-created` actor

You can compose several `react`s with same spec for many similar faces in window `on-created` actor, as e.g.
```
clear-reactions view/flags [
	on-created [
		win: face 
		reacts: copy [] 
		repeat i length? face/pane [
			append reacts compose/deep [
				react [
					(to-set-path compose [win pane (i) offset]) as-pair win/size/x - 10 * 1.0 / 3 * (i - 1) + 10 10 
					(to-set-path compose [win pane (i) size]) as-pair win/size/x - 10 * 1.0 / 3 - 10 win/size/y - 20
				]
			]
		] 
		do reacts
	] 
	box red box blue box green
] 'resize
```
And example with resizing fields:
```
clear-reactions view/flags [
	on-created [
		win: face 
		reacts: copy [] 
		repeat i length? face/pane [
			if face/pane/:i/type = 'field [
				append reacts compose/deep [
					react [
						(to-set-path compose [win pane (i) size]) as-pair win/size/x - 10 - win/pane/(i)/offset/x win/pane/(i)/size/y
					]
				]
			]
		] 
		do reacts
	]
	style lbl: text 50 right
	style fld: field 100
	lbl "First:" fld return
	lbl "Second:" fld return
	lbl "Third:" fld
] 'resize
```
# Is vs React

`React` is not meant to be used from inside a reactor, that's why it only recognizes paths are possible sources of reactions. That's what `is` is for. Basically, use `is` for "internal" relations inside a reactor, and `react` for "external" relations (requiring path accessors).

# Safety guidelines of v0.6.3
## Here's a handful of tricks a poor reactivist should always be wary of:

- Avoid circular relations.
- Avoid defining reactions during the object creation process: take them all out into a function and call it when the object is ready and it's initial conditions are properly set up. Otherwise depending on the build, your chances are the reaction will never be triggered or even created.
- Never use in a reactor a series that can reference itself, even after it's creation, and even it is neither a source nor a target. Better not use any series format of which you can't know in advance. It crashes. Kaboom!
- If the sources aren't ready, skip the initial reaction with the `react/later` (or `react later` in VID).
- Try to avoid `is` in favor of `react` for `is` might make 2 or 3 reactions where only one requested.
- Mind that in `react` you've got to specify full paths instead of words as sources.
- Do look at `dump-reactions` output after you've defined the reactions and double check.
- If you're changing 2 or more sources in a go and wanna minimize the number of times your target is recomputed, try `set-quiet` or in more complex scenarios use a boolean flag as a trigger that you negate when the source data is ready.
- If target is affected by some source indirectly (that is, not in the reaction formula itself but in the functions that it invokes), just mention these sources in the formula: `make reactor! [ x: 1  y: 2  f: does [1 + y: x * 2]  z: is [x y f] ]`
- Changing individual parts of composite scalar values (pair! tuple! time! etc) won't trigger the reaction: `[x: 0x0 y: is [x/x] x/x: 1]` and `[x: 0x0 y: is [x] x/x: 1]` are a no-go. Reassign the whole source or divide it into named components. With some scalars a `deep-reactor!` might also help.


# Future work

If you are interested in building a new, more sophisticated reactive library for Red (as suggested in https://github.com/red/red/issues/3096), you are welcome. The current one serves its purpose very well, but it might not be good enough for more complex use-cases. One difficult part would be designing a new API which is as lightweight as possible; ideally as simple as current one, but that may not be possible.

