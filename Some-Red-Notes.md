Author: Peter W A Wood

# char! datatype

A char! value is a Unicode code point in the range 00h to 10FFFFh. It can only hold values in this range.

The following maths operators are supported: +, -, *, /, and mod.

Calculations that would result in a char! value out of the supported range will be "wrapped" back into the range. For example:

    #"^(10FFFFh)" + 1 -> #"^(00h)"
    #"^(00h)" - 1 -> #"^(10FFFFh)"
