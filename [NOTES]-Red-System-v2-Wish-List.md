This page lists all the wishes various people have expressed for Red/System v2 (which will be rewritten in Red). There are three sections; Included, Requested, Rejected. The reviewing and selection process has not been done yet.

## Included
These features will be incorporated into Red/System V2.

## Requested
These features have been requested for inclusion in Red/System V2.

1. Add support for string comparison to `=` operator. `=` would be a case-insensitive comparison, while `==`could be used for a case-sensitive one. See [issue #8] (https://github.com/red/Red/issues/8).

2. The `=` and `==` operators should be used for almost-equal and strict-equal for float values.

3. stack-local struct values. See [issue #115] (https://github.com/red/Red/issues/115). 

4. Allow type casting chaining. See [issue #132] (https://github.com/red/Red/issues/132) and [issue #142] (https://github.com/red/Red/issues/142).

5. Add a `break` keyword for early escaping from loops. See [issue #166] (https://github.com/dockimbel/Red/issues/166).

6. Pointer to enum type name. See [issue #297] (https://github.com/red/Red/issues/297).

7. Allow byte! as index. See [issue #318] (https://github.com/red/Red/issues/318).

8. Replace #define pre-processor construct. The options are explained in [Pre-processor #define replacement](https://github.com/red/red/wiki/[ARCHIVE]-Alternatives-to-Red-System-pre-processor-%23define).

9. Allow empty struct! definitions. See [issue #194] (https://github.com/red/Red/issues/194).

10. Reverse the evaluation of function arguments. See [issue #213] (https://github.com/red/Red/issues/213).

11. Rename #enum to enumerate. See [issue #242] (https://github.com/red/Red/issues/242).

12. Allow function pointers to be passed as arguments. See [issue #259] (https://github.com/red/Red/issues/259).

13. Change 1-based indexing to 0-based. See [issue #264] (https://github.com/red/Red/issues/264).

14. Add casting from function![...] to logic!. See [issue #301] (https://github.com/red/Red/issues/301).

15. Extend DocStrings to Alias Struct! & #define. See [issue #329] (https://github.com/red/Red/issues/329).

16. Change index variable to get-word. See [issue #341] (https://github.com/red/Red/issues/341).

17. Introduce BREAK and CONTINUE keywords. See [issue #349] (https://github.com/red/Red/issues/349).

18. Introduce GOTO keyword. See [issue #350] (https://github.com/red/Red/issues/350).

19. Be able to get the address of a struct pointer, &aStructPointer.See [issue #351] (https://github.com/red/Red/issues/351).

20. Introduce a union type (a struct whose members share the same memory space). See [issue #352] (https://github.com/red/Red/issues/352).

21. EITHER expression allowed as an argument to a typed function call. See [issue #192] (https://github.com/red/Red/issues/192).

22. Add .rsrc entry in the data directory of the PE emitter. See [issue #471] (https://github.com/red/Red/issues/471).

23. Add 'FUNCTION definition in addition to 'FUNC. [Details] (https://github.com/red/red/wiki/[PROP]-Add-'FUNCTION-definition-in-addition-to-'FUNC).

24. Extend type declarations to include initial value. [Details] (https://github.com/red/red/wiki/[PROP]-Extend-type-declarations-to-include-initial-value).

25. Provide a simple way to initialise repeating characters in c-string! values. [Details] (https://github.com/red/red/wiki/[PROPOSAL]-Provide-a-simple-way-to-initialise-repeating-characters-in-strings).

26. Add array capabilities. See [ideal array capabilities] (https://github.com/red/red/wiki/[PROP]-Proposition-for-ideal-array-capabilities).

27. Formalise the support of local functions. See [local functions] (https://github.com/red/red/wiki/[NOTES]-Local-functions).

28. Allow constants to be defined within a namespace. [Details] (https://github.com/red/Red/wiki/[NOTES]-Namespace-constants).

29. Expose all standard numeric types. [Details] (https://github.com/red/Red/wiki/[PROP]-Expose-all-standard-numeric-types).

31. Optimise 'DOES. [Details] (https://github.com/red/Red/wiki/Optimised-'DOES).

32. Provide compile time option to re-define previously defined functions, etc for testing. [Details] (https://github.com/red/Red/wiki/Compile Time Option to Re-define Functions).

33. Allow aliases of simple types (integer!, byte!, etc.) to give finer control over type checking to the programmer. [Details] (https://github.com/red/Red/wiki/Aliases of Simple Types).

34. Provide a type? function to allow type checking of struct!s and alias!s passed to typed functions. [Details] (https://github.com/red/Red/wiki/Type?).

35. Add a symbol! type. [Details] (https://github.com/red/Red/wiki/symbol!).

36. Allow function overloading. [Details] (https://github.com/red/red/wiki/[PROP]-Function-overloading).

37. Additional struct! features. [Details] (https://github.com/red/Red/wiki/Additional-struct!-features).

38. Implement get-word for logic! values. [Details] (https://github.com/red/red/wiki/[NOTES]-Logic!-Get-Word). See [issue #534] (https://github.com/red/Red/issues/534).

39. Add binding to external variables. It would be nice to check C variables such as errno.

40. Function returns struct! by value (found in some C bindings). [Details] (https://github.com/red/red/wiki/[ARCHIVE]-Struct!-By-Value).

## Rejected
These features will not be incorporated into Red/System V2.