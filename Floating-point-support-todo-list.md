Todo list for supporting floating point datatype in Red/System:

1. Loader: extend it to accept decimal! type (using REBOL's LOAD native).
2. Add definitions in compiler for recognizing float!, float32!, float64! types.
3. Add FPU specific initialization routine (when required, like for IA-32).
4. 64-bit support in all stack-related calculations (currently limited to 32-bit)
5. Write a new 'emit-operation backend function for math operations involving floats.
6. Decide on how to handle NaN and INFs.
7. Add type conversion routines and float constants to runtime.
8. Add real support for decimal! in virtual-struct library (compiler helping lib).
9. Write enough unit tests to cover all edge cases.
