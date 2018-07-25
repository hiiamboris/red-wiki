Todo list for supporting floating point datatype in Red/System. The work on floats has been split in two branches, the `float-partial` branch has been almost finished, while the `float-full` branch has been postponed and not scheduled yet. The initial float support is meant for making possible the interfacing with some external libraries.

### float-partial branch

1. <strike>Loader: extend it to accept decimal! type (using REBOL's LOAD native).</strike> (<i>was not required finally</i>)
2. <strike>Add definitions in compiler for recognizing float!, float32!, float64! types.</strike>
3. <strike>Add FPU specific initialization routine (when required, like for IA-32).</strike>
4. <strike>64-bit support in all stack-related calculations (currently limited to 32-bit).</strike>
5. <strike>Add support for floats in structs.</strike>
6. <strike>Add support for floats in pointers.</strike>
7. <strike>Add support for floats in variadic and typed functions.</strike>
7. <strike>Write a new 'emit-operation backend function for math operations involving floats.</strike>
    1. <strike>Support float!/float! math ops.</strike>
    2. <strike>Support float32!/float32! math ops.</strike>
    3. <strike>Support float32! to float! type casting.</strike>
    4. <strike>Support float! to float32! type casting.</strike>
    5. <strike>Support integer! to float32! type casting.</strike>
    5. <strike>Support float32! to integer! type casting.</strike>
13. <strike>Add float datatypes description to language specification document.</strike>
12. <strike>Add float support to ARM backend.</strike>

### float-full branch (not planned yet)

2. Support mixed integers/floats math ops.
2. Improve IA-32 float support code relying more on `width` instead of types testing.
3. Add float support to `stack/push` and `stack/pop` functions.
8. <strike>Add read/write FPU and floats settings to `system` structure.</strike>
8. Provide a good Ulp-based `almost-equal` operator.
8. Decide on how to handle NaN and INFs.
9. Add type conversion routines to runtime.
10. Add float constants to runtime.
11. Add support for decimal! in virtual-struct library (compiler helping lib).
12. Write enough unit tests to cover all edge cases (NaNs, INFs, +/-0.0, comparisons,...)
13. <strike>Implement hard-float ABI support for ARM backend.</strike>
