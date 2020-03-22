Red is flexible. Unlike some languages where there's a single way to do something, Red allows you to use different ways to express your intent in the way you think best for a given context. This makes for a constant struggle against consistency and the value that provides. Not only that, but where some systems have a single way to access values, and all queries for attributes, facets, sub-values, or meta information must use that channel, Red does not. From a user's perspective, there is no distinction between a datatype's action being called directly, a `native!`, a `function!` (which may wrap either), or even user-defined `op!`s. Then there are general selectors and paths, where there is no difference in how you `select` values from a series where keys are entirely data-defined and path accessors that support only a fixed set of attributes to choose from. Even the words we use to describe those vary by context. For example, when talking about `face!` objects we call them "facets", but don't use that term for object "words", map "keys", `pick` "indexes", or reflection "fields".


# Selection

## Select

## Pick

`Pick` is an action, generally used against series values and with numeric indexes, but each type can support its own rules about what values may be selected. For example, `date!` values are not series, but support ordinal accessors that can be used with `pick` or via path notation.

# Reflection

Reflection provides details or meta information about a value, which is separate from its "data space" (for lack of a better term). Here's an example:
```
>> o: object [body: [body block]]
== make object! [
    body: [body block]
]
>> body-of o
== [body: [body block]]
>> o/body
== [body block]
```


## Reflect

`Reflect` is an action, so each type can support its own rules about what values may be selected and by what names. 

## Standard reflectors

These use `reflect` internally, simply wrapping the field (value) to select in function form.

```
     body-of         function!     Returns the body of a value that supports reflection.
     class-of        function!     Returns the class ID of an object.
     keys-of         function!     Returns the list of words of a value that supports reflection.
     reflect         action!       Returns internal details about a value via reflection.
     spec-of         function!     Returns the spec of a value that supports reflection.
     values-of       function!     Returns the list of values of a value that supports reflection.
     words-of        function!     Returns the list of words of a value that supports reflection.
```

# Accessors

`system/catalog/accessors`

# Naming

https://github.com/red/REP/issues/68

- consistency, context, and completeness
- `?` vs `-of` suffix
- avoiding name conflicts
- `of op!` (incl. Boris' examples)
