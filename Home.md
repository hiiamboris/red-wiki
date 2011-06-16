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
* <strike>C-strings indexes syntax and compilation support</strike>
* <strike>Structs members read & write access: implemented but broken</strike>
* <strike>Pointers `/value` syntax support</strike>
* <strike>Pointer paths with indexes support</strike>
* <strike>Pointer arithmetic</strike>
* <strike>Add c-string variables arithmetic</strike>
* <strike>Add struct variables arithmetic</strike>
* <strike>Functions call arguments type checking</strike>
* <strike>Functions return value type checking</strike>
* <strike>Type casting compilation warnings & errors messages</strike>
* <strike>Add type inference for functions local variables</strike>
* <strike>Check for function's two arguments when `infix` attribute is present</strike>
* <strike>Redefinition attempt check for reserved keywords</strike>
* <strike>Add "pointer" as reserved keyword</strike>
* <strike>Global variables proper initialization checking (should be initialized before used as argument)</strike>
* <strike>Hex values syntax checking</strike>
* <strike>Handle all errors in a consistent way</strike>
* <strike>Provide a way to write OS-specific code in user programs</strike>

Remaining features to implement (ordered by priority):

(none)

Additional low-level features to implement (ordered by priority):

* <strike>cdecl callbacks support</strike>
* <strike>Add runtime errors catching</strike>
* [PIC](http://en.wikipedia.org/wiki/Position-independent_code) code generation support
* Shared library generation support for current file emitters
* Extend #import to support importing values (ex: `#import ["libc.so" cdecl [stdin: "stdin" [value: [integer!]]]]`)