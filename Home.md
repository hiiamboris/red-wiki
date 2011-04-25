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

Remaining features to implement (ordered by priority):

* C-strings indexes syntax and compilation support
* Structs members read & write access: implemented but <strike>untested</strike>broken
* Pointers `/value` syntax support
* Pointer paths with indexes support
* Pointer arithmetic
* Add struct variables arithmetic
* Add c-string variables arithmetic
* Functions call arguments type checking
* Functions return value type checking
* Add type inference for functions local variables and return value
* Type casting compilation warnings & errors messages
* Redefinition attempt check for reserved keywords
* Add "pointer" as reserved keyword
* Global variables proper initialization checking (should be initialized before used as argument)
* Hex values syntax checking
