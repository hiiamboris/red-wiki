When starting with a new language, there will be new things to learn, and things you expect to work the way you're used to, but don't. 

# Words evaluate by default

```
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

# Only `false` and `none` are falsey

Everything else, 0, empty strings, etc. is truthy.
