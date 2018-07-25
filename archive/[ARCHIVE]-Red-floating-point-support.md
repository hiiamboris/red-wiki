This is an exhaustive todo-list for adding 64-bit IEEE754 floating point support to Red. 

1. Decide on the datatype name: real!, float! or decimal!
2. Add a proper float datatype definition
3. Add parsing rules for a float datatype (with support to scientific notation E)
4. Implement an accurate cross-platform string to float conversion algorithm (for LOAD action)
5. Implement an accurate cross-platform float to string conversion algorithm with proper rounding (for FORM/MOLD actions)
6. Implement integer <=> float conversion routines (TO action)
7. Implement float/float comparison routines with proper rounding 
8. Implement integer/float comparison routines with proper rounding
9. Decide on the handling of NaNs, INFs and signed zeros.

Some of the following underlying Red/System [features](https://github.com/red/red/wiki/Floating-point-support-todo-list-(Red-System)) need also to be implemented for the above tasks.