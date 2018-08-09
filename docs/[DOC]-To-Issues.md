## Purpose

The purpose of this Wiki page is to facilitate resolving issues relating to value conversion from one datatype! to another. If used properly, it will also document the decisions made.

## All Datatypes

### Script Error or None?

This perennial question should be asked so that it is clear that a conscious decision has been made. If a value conversion cannot be made as requested should a script error or none be returned by the to function?

### Compiler Errors?

Should the compiler raise an error when it can be determined that a conversion cannot be made?

## Action!

### to series! 

Rebol 2 returns variations on "action" in series! datatypes. Wouldn't it be better to return either none or an error?

## Integer!

### to binary!

Rebol 2 molds the integer and then makes a binary from a molded integer. Wouldn't creating a binary of the actual integer be better? It would be very useful for debugging. Anybody really wanting the current behaviour would simply have to code - [to binary! mold i].

If this is adopted, a decision would need to be made to convert the integer to big endian format or not.

### to char!

Rebol2 treats char! as an unsigned single-byte integer. Its conversion rules reflect this. A char! in Red is a Unicode code point. The underlying implementation is a 32-bit unsigned integer with a restricted value range of 0h to 10FFFFh.

The logical conversion to perform is to create a code point from the integer where the integer is in the range 0h to 10FFFFh. Values outside that range should not be converted.

### to email!

Rebol2 molds the integer and then makes an email! value from the molded integer. This seems unnecessary as all integer email address probably don't exist in the real world. Anybody really wanting to create such an email value could simple write [to email! mold i].

### to file!

Rebol2 molds the integer and then makes a file! value from the molded integer. This seems unnecessary as all integer file names are very rare. Anybody really wanting to create such an file value could simple write [to file! mold i].

### to issue!

Rebol2 molds the integer and then makes an issue! value from the molded integer. 

This seems correct behaviour though it cannot be supported if an issue! is of the word! type. 

Whilst this may not be the correct place to discuss that issue, the real world use of issues is for issue numbers such as serial numbers, part numbers and revision numbers. This cannot be represented if issue! is a word! type.

### to logic!

Rebol 2 converts 0 to False and all other integers to True. This hints at an underlying implementation of logic! values that may or may not be true. It is inconsistent with Rebol2 implicit value conversions:
```red
>> if 0 [print "oh, oh"]
oh, oh
```
Conversion between integer! and logic! should not be supported.

### to tuple!

Should it be possible to convert an integer to a tuple? If so what should be the length of the tuple? Should an arbitrary length of 3 be used?

Rebol2 imposes a value range of 0 - 255 for each element of a tuple. Will Red do the same?

### to typeset!

Rebol2 performs to block! in this instance. 

Conversion between integer! and typeset! should not be supported.

### to url!

Rebol2 molds the integer and then makes a url! value from the molded integer. This seems unnecessary as all integer urls probably don't exist in the real world. Anybody really wanting to create such a url value could simple write [to url! mold i].