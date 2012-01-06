Todo list for supporting floating point datatype in Red/System:

1. Loader: extend it to accept decimal! type (using REBOL's LOAD native).
2. Add definitions in compiler for recognizing float!, float32!, float64! types.
3. Add FPU specific initialization routine (when required, like for IA-32).
4. 64-bit support in all stack-related calculations (currently limited to 32-bit)
5. Write a new 'emit-op backend for math operation involving floats.
6. Decide on how to handle NaN and INFs.
7. Add type conversion routines and float constants to runtime.
8. Add real support for decimal! in virtual-struct library (compiler helping lib).
9. Write enough unit tests to cover all edge cases.

The complex and longest part to implement is probably #5, it will take at least a week or two to make it work corectly on IA-32 and ARM. The rounding handling should also take significant time to handle well.

The rest should be done in a couple of days (except for #9), with maybe a day or two more for researching on IEEE754.

Also, you can add another week for fine-tuning and debugging.