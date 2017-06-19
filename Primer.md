When starting with a new language, there will be new things to learn, and things you expect to work the way you're used to, but don't. 

# Words evaluate by default

```Red
>> 'a
== a
>> a
*** Script Error: a has no value
*** Where: catch
>> :a
>> a:
*** Script Error: a: needs a value
*** Where: a
>> type? :a
== unset!
```
# Words are evaluated before they are passed to a function.
For example: `value? bflmpsv` will throw an error, because bflmpsvz gets evaluated and the result of the evaluation is passed to `value?`
```Red
>> value? bflmpsv
*** Script Error: bflmpsv has no value
*** Where: value?
*** Stack:
```

Check for unset! with `get/any`:
```Red
>> type? get/any 'bflmpsvz
== unset!
```
If you want to catch general "no value" errors, use `attempt`:
```Red
>> attempt [value? sdfdfsfg]
== none
```

# Only `false` and `none` are falsey

Everything else, 0, empty strings, etc. is truthy. In addition, `to` and `make` don't have the same semantics when creating logic values. e.g.:

```Red
>> to logic! 0
== true
>> make logic! 0
== false
```

# `Return:` typeset is `any-type!`, not `default!`

`Default!` doesn't include `unset!`.

# Series types should usually be copied inside functions

Otherwise your function may behave differently on each call. See the topic: [[Why do I have to copy series values?]]
