See also: [Error Messages](https://github.com/red/red/wiki/%5BNotes%5D-Error-Messages)

# Error values

## error!

## Standard Errors

See `system/catalog/errors` for standard error definitions

## User Defined Errors


# Creating Errors

## Make error!

Making errors with `make error!` ...

To dispatch an error you have to `do` it:

```Red
do make error! "This is an error"
```

## `cause-error`

`Cause-error` is a wrapper for `do make error!`

```Red
cause-error <err-type> <err-id> <args>
```
Where:
* `<err-type>` is a word from `words-of system/catalog/errors`.
* `<err-id>` is a word for error message template from `words-of system/catalog/errors/<err-type>`
* `<args>` is a block of values that will be inserted in selected error template.

Example:
```Red
>> cause-error 'internal 'bad-path ['foo/bar]
*** Internal Error: bad path: foo/bar
*** Where: do
*** Stack: cause-error

>> cause-error 'internal 'no-memory []
*** Internal Error: not enough memory
*** Where: do
*** Stack: cause-error  
```
```Red
>> probe system/catalog/errors/internal
make object! [
    code: 900
    type: "Internal Error"
    bad-path: ["bad path:" arg1]
    not-here: [arg1 "not supported on your system"]
    no-memory: "not enough memory"
    wrong-mem: "failed to release memory"
    stack-overflow: "stack overflow"
    too-deep: "block or paren series is too deep to display"
    feature-na: "feature not available"
    not-done: "reserved for future use (or not yet implemented)"
    invalid-error: ["invalid error object field value:" :arg1]
    routines: {routines require compilation, from OS shell: `red -r <script.red>`}
    red-system: {contains Red/System code which requires compilation}
]
```

Problem: there's no simple and free-form way to throw user errors.
Even wrapper on `cause-error 'user 'message [...]` is too limited:
```Red
>> cause-error 'user 'message ["User message, still with quotes"]
*** User Error: "User message, still with quotes"
*** Where: do
*** Stack: cause-error
```
* Error message is displayed with quotes
* `Where` and `Stack` aren't informative enough.


# Handling Errors

## error?

## try

## attempt


# Non-local Flow Control

## Catch

## Throw


# Errors in the console


# Differences from Rebol

## R2

## R3

# References

http://helpin.red/Errorhandling.html

http://www.red-by-example.org/index.html#cat-e01

https://github.com/red/red/issues/3592