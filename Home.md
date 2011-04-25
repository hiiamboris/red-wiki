Welcome to the Red wiki!

### Red/System Todo-list

This section lists all the remaining features to implement to conform to
the specification draft: [[http://www.red-lang.org/p/documentation.html]]


* <strike>Char! literals as input format for integer! values</strike>
* <strike>Bind char! literals to byte! datatype</strike>
* <strike>Support pointers literal values</strike>
* <strike>Rename string! to c-string!</strike>
* <strike>Rename "alias-type" keyword to "alias"</strike>
* <strike>Get-word! syntax for getting function address</strike>
* <strike>EXIT and RETURN function specific keywords</strike>
* <strike>Modulo (`//`) operator</strike>
* <strike>Add byte! datatype</strike>
* <strike>Add logic! datatype</strike>
* <strike>Add TRUE and FALSE keywords</strike>
* <strike>Add NOT operator</strike>

Remaining tasks (ordered by priority):

1. C-strings indexes syntax and compilation support
1. Structs members read & write access: implemented but untested
1. Pointers `/value` syntax support
1. Pointer paths with indexes support
1. Pointer arithmetic
1. Add struct variables arithmetic
1. Add c-string variables arithmetic
1. Functions call arguments type checking
1. Functions return value type checking
1. Add type inference for functions local variables and return value
1. Type casting compilation warnings & errors messages
1. Redefinition attempt check for reserved keywords
1. Add "pointer" as reserved keyword
1. Global variables proper initialization checking (should be initialized before used as argument)
1. Hex values syntax checking
