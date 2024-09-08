# Table of Contents

1. [`copy` on `object!`](#copy-on-object)
1. [`float!` vs `decimal!`](#float-vs-decimal)
1. [`function` vs `funct`](#function-vs-funct)
1. [Local contexts for loops](#local-contexts-for-loops)
1. [Binding to `self`](#binding-to-self)
1. [Invalid block selector returns `none`](#invalid-block-selector-returns-none)
1. [Reflection](#reflection)
1. [`dir?`](#dir)
1. [`to-date`](#to-date)
1. [`random`](#random)
1. [`do`](#do)
1. [`if`](#if)
1. [`make` with `none`](#make-with-none)
1. [`last`](#last)
1. [`take`](#take)
1. [`to-integer`](#to-integer)
1. [`load`](#load)
1. [`read`](#read)
1. [Adjust time by setting timezone](#adjust-time-by-setting-timezone)
1. [`repend`](#repend)
1. [`parse`](#parse)
1. [`to-time`](#to-time)
1. [`select/skip`](#selectskip)
1. [Refinements](#refinements)
1. [Lexer](#lexer)
1. [`to-logic`](#to-logic)
1. [`get`](#get)
1. [`set`](#get)
1. [`equal?`](#equal)
1. <code><a href="#what-what">??</a></code>
1. [`all`](#all)
1. [`panel`](#vid-panel)
1. [`system/options/boot`](#systemoptionsboot-is-string-not-a-file)
1. [`compress and decompress`](#compress-and-decompress)

## `copy` on `object!`

* R2: not supported.
* R3: does not rebind functions to the new copy.
* Red: rebinds functions to the new copy.

Example:

```red
>> o1: make object! [a: 42 m: does [a]]
>> o2: copy o1
>> o2/a: 99

>> o1/m
== 42         ;; Both R3 and Red
    
;; R3 does not rebind:
>> o2/m       ;; R3
== 42         ;; R3

;; Red rebinds:
red>> o2/m    ;; Red
== 99
```

## `float!` vs `decimal!`

Different names for datatype implementing the [standard IEEE-754 64-bit binary floating-point format](http://en.wikipedia.org/wiki/Double-precision_floating-point_format).

* R2 & R3: `decimal!`<sup>[1](http://www.rebol.com/r3/docs/datatypes/decimal.html)</sup>
* Red: `float!`<sup>[2](http://www.red-lang.org/2014/08/043-floating-point-support.html)</sup>

## `function` vs `funct`

R2:
- `function` is a 3-argument function constructor taking a `spec` block, a `vars` (locals) block, and a `body` block. It is a mezzanine function.
- `funct` is a 2-argument function constructor taking a `spec` block, and a `body` block. `set-word!`s in `body` are automatically (and deeply) collected as `/local`s of the function. It is a mezzanine function.

R3:
- `function` is a 2-argument auto-localising mezzanine; collects `set-word!`s only.
- `funct` is an alias for `function`.

Red:
- `function` is a 2-argument auto-localising native; collects `set-word!`s and words of iterators `foreach` and `repeat`.
- `funct` does not exist.

## Local contexts for loops

* R2 & R3: loops with words for counters or iterated values have a local context for them.
* Red: does not provide local contexts for loops, due to its high runtime cost: it requires rebinding (and eventually deep copying of the whole body block) each time the loop is about to be evaluated.

## Binding to `self`

* R2 & R3: either `'self` or `self` can be used for binding words to the context of an object.
* Red: only `self`.

## Invalid block selector returns `none`

In Rebol, usage of invalid `word!` selector in path throws an error, while in Red it returns `none`.

Rebol:
```rebol
>> a: [] a/b
** Script Error: Invalid path value: b
** Where: halt-view
** Near: a/b
```

Red:
```red
>> a: [] a/b
== none
```

## Reflection

Historically, reflection on `object!` values in R2 was done with `first`, `second` and `third` functions, which return words, values and body of a given object, respectively. For `any-function!` values, `first` returns argument words and refinements, `second` returns body or `action!` / `native!` ID, and `third` returns spec.

Later versions included mezzanine reflectors `words-of`, `values-of`, `body-of` and `spec-of` for the same purposes, but retained previous conventions.

In R3, only `*-of` reflectors can be used, and `self` with object's back-reference are excluded from object's words and values blocks; `body-of` on `native!` and `action!` values yields `none`.

Red follows R3 convention, with an exception that `body-of` on `native!`s and `action!`s results in an internal error and `words-of` on `any-function!` values is not defined.

Both in R3 and Red `*-of` reflectors are wrappers on top of dedicated `reflect` action.

## `dir?`

In R2, `dir?` returns `true` if given directory exists. In Red, it returns `true` if provided value syntactically looks like a directory, i.e. ends with a slash.

## `to-date`

In Rebol, `to-date` function works with string values, but in Red this is currently not supported (note that this is subject to change). In either case, `load` can be used instead of string conversion.

## `random`

In R2, `random` on series shuffles its copy, but in R3 and Red series is modified in-place.

## `do`

The values that get special treatment by `do` are: `[block! path! string! url! file! error!]`. Everything else is evaluated passively. This is by design, to eliminate variable arity (see [here](https://rebol.tech/gitter.im/red/help/2018/#msg5ab285275f188ccc15df954b)).

## `if`

`if` in Red has no `/else` (R2, equivalent to `either`) and `/only` (R3, returns `block!` branch as-is) refinements.

## `make` with `none`

In R3 and Red, `make` generally doesn't accept `none` in spec argument and raise an error, which is different from R2:

```rebol
make integer! none ; == 0
make block!   none ; == []
make file!    none ; == %""
make tag!     none ; == <>
...
```

Same applies to empty string:

```rebol
make integer! "" ; == 0 (R2)
make path!    "" ; <empty path>
...
```

Also, there's a difference in `hash!` behavior:

```red
length? make hash! none
```

This returns `0` in R2 and `1` in Red.

## `last`

`last` on empty series returns `none` in Red and R3, but yields an error in R2.

## `take`

`take` on empty series returns `none` both in Red and Rebol, but `take/part ... 1` returns empty series in Rebol and `none` in Red.

## `to-integer`

`to-integer ""` returns error in Red and R3, but `0` in R2.

See also: FIX: issue [#5147](https://github.com/red/red/issues/5147) (Question on the behavior of parsing integers).

## `load`

1. `load` on `file!` loads file's content and return either one value or a block of values in Red and R2. In R3 it returns a string:

```red
>> load %/c/file.txt
== [abc def] ; Red & R2
== "abc^/def^/" ; R3
```

2. `load %./` returns a block of files and folders in R2 and R3, but not in Red:

```red
>> load %/c/
== [%$Recycle.Bin/ %$WINDOWS.~BT/ %Boot/ %bootmgr ... ] ; R2 & R3
*** Script Error: transcode does not allow block! for its <anon> argument ; Red
```

## `read`

`read/lines/part` behaves differently between Red and Rebol:

```red
>> write/lines %file.txt ["one" "two" "three"]

>> read/lines/part %file.txt 2
== ["on"] ; Red & R3
== ["one" "two"] ; R2

>> read/lines/part %file.txt 4
== ["one^M"] ; Red
== ["one"] ; R3
== ["one" "two" "three"] ; R2
```

Note that this might change after 0.7.0 release.

## Adjust time by setting timezone

Setting timezone of `date!` value with `zone` won't adjust time in either language. But in Red, such adjustment works with `timezone`: 

```red
>> d: now
== 22-Dec-2018/1:53:52+03:00
>> d/timezone: 5:0:0
== 5:00:00
>> d
== 22-Dec-2018/3:53:52+05:00
```

## `repend`

`repend` in Rebol is just a shortcut for the common `append ... reduce` pattern. This means that you still incur the cost of an extra block produced by `reduce` before `append`.

In Red, `repend` is the fusion of `append` and  `reduce`, so that no intermediary block is created (thanks to the efficient `reduce/into` call). This means that values are reduced and appended one by one in this case. It will behave the same way as `append ... reduce`, unless you rely on side-effects, like above modifying the accumulating series while reducing. In such cases, just fall back on `append ... reduce` in order to separate fully the reduction from the appending actions.

See the details [here](https://github.com/red/red/issues/3340).

## `parse`

`path!`s are evaluated in R2 and R3 but not in Red:

```red
>> b: [x 3]
>> parse "aaa" [b/x "a"]
== true (in R2 & R3)
== false (in Red)
```

* Related ticket: [#3528](https://github.com/red/red/issues/3528)
* Also check `to` / `thru` difference described in [#3679](https://github.com/red/red/issues/3679)

---

Integer after range values work in R2 and R3, but not supported in Red:

```
>> parse [9] [1 1 9]
== true (R2 & R3)
*** Script Error: PARSE - invalid rule or usage of rule: 9 (Red)
```

## `to-time`

In Red, Meridian designations are not part of the `time!` form:
```red
>> load "01:00PM"
== [1:00:00 PM]        ;Red
== 13:00               ;R2 & R3

>> load "13:00PM"
== [13:00:00 PM]       ;Red
== Invalid time error  ;R2 & R3
```

See [specs](https://doc.red-lang.org/en/datatypes/time.html#_literal_syntax)

This leads to below differences:

```red
>> to time! "1:00PM"
== 01:00:00 ;Red
== 13:00    ; R2
== 13:00    ;R3
```

```red
>> to time! "13:00PM"
== 13:00 ;Red
== none  ;R2
== Error ;R3
```

## `select/skip`

`select/skip` returns a block (the record, see `? select`) in R2 and a single value in R3 & Red:

```red
>> select/skip [1 2 3 4 5 6] 1 3
== [2 3] ;R2
== 2     ;R3 & Red
```

## Refinements

Default value for refinements in Red is `false`, while in Rebol it's `none`.

`refinement!` is currently not part of `any-word!` as in Rebol:

```red
>> any-word? /ref
== true  ;R2 & R3
== false ;Red
```

## Lexer

Red's and Rebol's lexers behave differently in some cases:

```red
>> [/:a]
== [/ :a] ;R2 & R3
== [/: a] ;Red
```

## `to-logic`

`to-logic 0` returns `true` in Red and R3, but `false` in R2.

Some people think zero should be falsey, based on how many other languages do it, and *their* instinct was to follow how C did it. But C has no concept of real logic values. So what we need to ask is what makes the most sense in Red (Red/System is a different story, because it's a C level language).

Red chose to be consistent in how logic values are coerced, with only false and none mapping to false. Zero is a number, and no special case is made for it. Even `unset!` coerces to `true`.

That said, you have an option. `Make` creates a logic value of `false` if the spec is a numeric zero, including floats and percents.

## get

Given
```red
o: make object! [a: 1]
```

|             | Red         | R2          | R3     |
|:------------|:-----------:|:-----------:|:------:|
| `get o`     | `[1]`       | `[1]`       | `[1]`  |
| `get :o`    | `[1]`       | `[1]`       | `[1]`  |
| `get o/a`   | *error*     | *error*     | `1`    |
| `get 'o/a`  | `1`         | *error*     | `1`    |
| `get :o/a`  | *error*     | *error*     | `1`    |
| `get 1`     | *error*     | *error*     | `1`    |
| `get none`  | *error*     | `none`      | `none` |

## `set`

Rebol has `/pad` refinement that sets remaining words to `none`, but in Red that's a default behavior; `/some` can be used to mimick Rebol convention and ignore remaining words.
```red
>> set/some [a b] [1] reduce [:a :b]
== [1 unset]
>> set [a b] [1] reduce [:a :b]
== [1 none]
```

`/any` works the same, and newly present `/case` with `/only` act according to their specs:
```red
>> attempt [a: 1 set 'a () ?? a]
== none
>> attempt [a: 1 set/any 'a () ?? a]
a: unset!

>> block: [a 1 A 2] set 'block/A 3 block
== [a 3 A 2]
>> block: [a 1 A 2] set/case 'block/A 3 block
== [a 1 A 3]

>> set [a b][1 2] reduce [a b]
== [1 2]
>> set/only [a b][1 2] reduce [a b]
== [[1 2] [1 2]]
```

## `equal?`

`equal?` accepts `unset!` in Red and R3, but not in R2.

```red
>> equal? () ()
== true (Red & R3)
** Script Error: equal? is missing its value1 argument (R2)
```

<a id="what-what"/>

## ??

In R2 & R3, `??` takes a literal value and returns it back, so that it can be used inside expressions for debugging:
```
>> 5 - ?? 2
2
== 3
```
In Red, `??` returns `unset` and only accepts words and paths:
```red
>> ?? 1
*** Script Error: ?? does not allow integer! for its 'value argument
*** Where: ??
*** Stack: ?? 
```

The rationale behind `unset` is that Rebol console by default hid most of the values and one had to use `??` to see them:
```
>> f: does [something]
>> o: make object! [a: 1]
>> :f
>> o
>> ?? f
f: func [][something]
>> ?? o
o: make object! [
    a: 1
]
```

while Red console displays all the values:
```red
>> f: does [something]
== func [][something]
>> :f
== func [][something]
```

So `??` returning them would have led to output duplication, like this:
```red
>> ?? o
o: 
make object! [
    a: 1
    b: 2
    c: 3]
== make object! [
    a: 1
    b: 2
    c: 3
]
```

From within *Red expressions* `probe` should be used instead:
```red
>> 1 + probe 2
2
== 3
```

## all

```red
>> all []
== none ; in Red
== true ; in R2 & R3
```
## vid panel

* R2: Any `layout` can be used as a subpanel

* Red: Use `layout/only` to create subpanels (or only use the contents of the subpanel's pane)

This code - simplified from Rebol's How-To page for subpanels (http://www.rebol.com/how-to/subpanels.html
) - does not work in Red:

```
main: layout [
    h1 "Subpanel Examples"
    button "Panel 1" [panels/pane: panel1 show panels]
    button "Panel 2" [panels/pane: panel2 show panels]
	return
    panels: panel 220x140 []
]
panel1: layout [h2 "Panel 1"]
panel2: layout [h2 "Panel 2"]
view main
```

The two alternative ways to fix it are shown here:
```
main: layout [
    h1 "Subpanel Examples"
    button "Panel 1" [panels/pane: panel1/PANE show panels]  ; fixed with PANE
    button "Panel 2" [panels/pane: panel2 show panels]
	return
    panels: panel 220x140 []
]
panel1: layout [h2 "Panel 1"]
panel2: layout/ONLY [h2 "Panel 2"]   ; fixed with /ONLY
view main
```

# `system/options/boot` is `string!` not a `file!`

Yes. For now. %environment/functions.red@extract-boot-args simply breaks up the args from the OS but doesn't convert them to Red values. Red follows R2 here, with the exception that it keeps options/boot consistent with the rest of them, by not changing it. Options/path, OTOH, is changed by change-dir, so it is a Red file.

That said, it's something we may revisit. R2's design here confused people more than once. It's true that system/script/args is just the raw string, which you can load, but the naming doesn't make it clear for a feature that should be used quite heavily. Since they current options don't limit you, the way we'll probably deal with this is to hide them behind a CLI dialect.

# compress and decompress

Data compressed in R2 cannot be decompressed in Red.

rgchris' explanation:

R2 Rebol's COMPRESS is Deflate compressed data with a non-standard wrapper:

```
| 2 byte header | payload | 4 byte checksum | 4 byte size |
```

If you skip the header and run decompress next next your-file-content 'deflate, Red will ignore the final eight bytes so long as the payload is in valid Deflate format.

See the discussion here (https://matrix.to/#/!wUTlqkqOhNGtfQzIsO:matrix.org/$169685816231IlJgS:gitter.im?via=gitter.im&via=matrix.org&via=tchncs.de)

R2's `COMPRESS` produces a ZLIB envelope with the additional 4-byte uncompressed size value tacked on (hence non-standard). If you remove the size value then you can `DECOMPRESS` using 'zlib.

```red
decompress head clear skip tail your-file-content -4 'zlib

decompress read/part/binary file-name -4 + size? file-name 'zlib
```

