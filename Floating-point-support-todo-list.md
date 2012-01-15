Todo list for supporting floating point datatype in Red/System:

1. <strike>Loader: extend it to accept decimal! type (using REBOL's LOAD native).</strike> (<i>was not required finally</i>)
2. <strike>Add definitions in compiler for recognizing float!, float32!, float64! types.</strike>
3. <strike>Add FPU specific initialization routine (when required, like for IA-32).</strike>
4. <strike>64-bit support in all stack-related calculations (currently limited to 32-bit).</strike>
5. <strike>Add support for floats in structs.</strike>
6. <strike>Add support for floats in pointers.</strike>
7. Write a new 'emit-operation backend function for math operations involving floats.
8. Decide on how to handle NaN and INFs.
9. Add type conversion routines to runtime.
10. Add float constants to runtime.
11. Add real support for decimal! in virtual-struct library (compiler helping lib).
12. Add float support to ARM backend.
13. Add float datatypes description to language specification document.
12. Write enough unit tests to cover all edge cases.
