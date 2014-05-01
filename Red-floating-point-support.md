This is an exhaustive todo-list for adding 64-bit IEEE754 floating point support to Red. 

1. Decide on the datatype name: real!, float! or decimal!
2. Add a proper float datatype definition
3. Add parsing rules for a float datatype (with support to scientific notation E)
4. Implement an accurate cross-platform string serialization algorithm with proper rounding (for FORM/MOLD actions)
5. Implement integer <=> float conversion routines (TO action)
6. Implement float/float comparison routines with proper rounding 
7. Implement integer/float comparison routines with proper rounding
8. Decide on handling NaNs and INFs

Some of the following underlying Red/System [features](https://github.com/red/red/wiki/Floating-point-support-todo-list) need also to be implemented for the above tasks.