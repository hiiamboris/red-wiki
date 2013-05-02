This page lists all the features we would like to add to Red/System v2, which will be rewritten in Red. There are three sections; Included, Requested, Rejected.

## Included
These features will be incorporated into Red/System V2.

## Requested
These features have been requested for inclusion in Red/System V2.

1. Add support for string comparison to `=` operator. `=` would be a case-insensitive comparison, while `==`could be used for a case-sensitive one. See [issue #8] (https://github.com/dockimbel/Red/issues/8).

2. The `=` and `==` operators should be used for almost-equal and strict-equal for float values.

3. stack-local struct values. See [issue #115] (https://github.com/dockimbel/Red/issues/115). 

4. Allow type casting chaining. See [issue #132] (https://github.com/dockimbel/Red/issues/132) and [issue #142] (https://github.com/dockimbel/Red/issues/142).

5. Add a `break` keyword for early escaping from loops. See [issue #166] (https://github.com/dockimbel/Red/issues/166).

6. Pointer to enum type name. See [issue #297] (https://github.com/dockimbel/Red/issues/297).

7. Allow byte! as index. See [issue #318] (https://github.com/dockimbel/Red/issues/318).

8. Replace #define pre-processor construct. The options are explained in [Pre-processor #define replacement](https://github.com/dockimbel/Red/wiki/Alternatives-to-Red-System-pre-processor-%23define).

9. Allow empty struct! definitions. See [issue #194] (https://github.com/dockimbel/Red/issues/194).

10. Reverse the evaluation of function arguments. See [issue #213] (https://github.com/dockimbel/Red/issues/213).

11. Rename #enum to enumerate. See [issue #242] (https://github.com/dockimbel/Red/issues/242).

12. Allow function pointers to be passed as arguments. See [issue #259] (https://github.com/dockimbel/Red/issues/259).

13. Change for 1-based indexing to 0-based. See [issue #264] (https://github.com/dockimbel/Red/issues/264).

14. Add casting from function![...] to logic!. See [issue #301] (https://github.com/dockimbel/Red/issues/301).

15. Extend DocStrings to Alias Struct! & #define. See [issue #329] (https://github.com/dockimbel/Red/issues/329).

16. Change index variable to get-word. See [issue #341] (https://github.com/dockimbel/Red/issues/341).

17. Introduce BREAK and CONTINUE keywords. See [issue #349] (https://github.com/dockimbel/Red/issues/349).

18. Introduce GOTO keyword. See [issue #350] (https://github.com/dockimbel/Red/issues/350).

19. Be able to get the address of a struct pointer, &aStructPointer.See [issue #351] (https://github.com/dockimbel/Red/issues/351).

20. Introduce a union type (a struct whose members share the same memory space). See [issue #352] (https://github.com/dockimbel/Red/issues/352).


## Rejected
These features will not be incorporated into Red/System V2.