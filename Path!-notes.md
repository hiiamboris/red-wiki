`Path!` values are special in many ways. They are a block type, but don't require any bracketing characters to enclose them. Instead, they use the `/` character as an element separator. This is natural, and applied widely in Red. When a path is evaluated, as when accessing nested elements in structures, or when calling functions, the type of value in each slot (sometimes called a _segment_ when discussing paths) controls evaluation. That is, words, integers, get-words, and parens can be used to control literal and parameterized values in path segments.

But what happens in other cases? Red has a _lot_ of datatypes. How does each of them behave when used in a `path!`? 

## `path!` values in slots

Can you have a path within a path? Yes, you can. Look at this example:

```
>> p1: to path! [a b c]
== a/b/c
>> p2: to path! [a b/c]
== a/b/c
>> p1 = p2
== false
>> mold p1
== "a/b/c"
>> mold p2
== "a/b/c"
>> length? p1
== 3
>> length? p2
== 2
>> p1/2
== b
>> p2/2
== b/c
>> to block! p1
== [a b c]
>> to block! p2
== [a b/c]

>> to path! reduce ['b to block! 'name/phone]
== b/[name phone]
```

The two path values look the same when rendered, but are not the same internally. We have the same behavior in many other datatypes in Red:

```
>> word! = first [word!]
== false
>> none = first [none]
== false
```

The lexical space used by Red is not big enough to represent all the possible values you can construct at runtime (even construction syntax cannot represent everything, like different bindings for the same word), so we can end up with overlapping text representations. That's a fact of life in Red's design. We can address it in various ways (e.g., syntax highlighting), but it's important to know that it exists. 

To be continued...