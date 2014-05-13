## Introduction
There are a number of issues in using Unicode text encoding. This page gives a brief overview of the issues and the proposed resolution in Red.

## Encoding Normalisation
Many printable characters can be encoded in more than one way in Unicode. They may be encoded in a single code point (generally called a precomposed character) or a sequence of code points (decomposed) For example, whilst in ISO 8859-1 the letter 'ç' can only be represented as the single character E7 'ç', but in a Unicode encoding it can be represented as the single code point U+00E7 'ç' or the sequence U+0063 'c' U+0327 '¸'. A fuller understanding of the issue may be gained by reading this [Unicode.org technical report](http://unicode.org/reports/tr15/) and this [W3C paper](http://www.w3.org/TR/charmod-norm/).

Clearly for comparisons and sorting, Unicode encoded text must use a consistent encoding for each printable character. Unicode text can be "normalised" to use consistent encoding. There are in fact, four standard normalisations; two based on precomposed characters and two based on decomposed characters.

This is a real, not a theoretical, problem. Unicode text originating from Linux systems generally conforms to a precomposed normalisation whilst text originating from OS X systems conforms to a slightly non-standard decomposed normalisation.

### Encoding Normalisation Proposal
The choice of whether to normalise Unicode text and the method of normalisation is left to the user programmer. Built-in functions will be provided to normalise Unicode strings to one of the four standard normalisations.

## Case Folding (Sensitivity)
The issue with case folding and case sensitivity is that it is language (or "locale" in Unicode speak) dependent. A simple example, demonstrates this. In English, the lower case equivalent of I (+U0049) is i (+0069). In Turkish, the lower case equivalent of I (+U0049) is ı (+U0131). For a fuller explanation read this Unicode.org [technical report](http://unicode.org/reports/tr21/tr21-5.html).

There are a few other mapping special cases special cases, such as in German the capital of ß being SS whilst a capital ß is defined in Unicode.

### Case Folding Proposal
Red will provide the standard Unicode Case Mapping as defined in [this Unicode.org table](http://www.unicode.org/Public/3.2-Update/CaseFolding-3.2.0.txt) defaulting to English as the language where necessary.

Red will enable other case mappings by providing a simple "plug-in" mechanism for other specific case mappings.

##One "Character", Multiple Code Points 
It is easy to fall into the trap that a Unicode Code Point is the equivalent of a printable character. This is clearly not the case as demonstrated by the simplistic example in the Normalisation section. Within the Unicode standard many Code Points may be needed to represent a displayable character. These multiple code points are known as a "grapheme cluster".

An example of the type of issues that this aspect of Unicode bring is the length? function. It returns the number oeme cluster and the start of the next is not easy. Basically, you have to work out if the next code point is the start of a new grapheme cluster. The [Unicode technical report on text segmentation](http://www.unicode.org/reports/tr29) contains all the gory details. 

It should also be noted that finding word, sentence and paragraph breaks are equally challenging.
f code points in a string not the number of displayable characters. Reversing strings containing multi-code point grapheme clusters will produce incorrect results. For example, the string "ç", if encoded by the sequence U+0063 U+0327 when reversed would become "¸c".

Determining the end of one graph
###One Character, Multiple Code Points Proposal.
There is substantial extra processing in providing full "grapheme cluster" support that will not be used in the vast majority of programs written in Red. As a result, it is proposed not to change the current default behaviour from treating code points individually.

However, given that many other languages provide very full Unicode support, often based on the Internationalisation Components of Unicode libraries, Red needs to include support for "grapheme cluster" processing.

It is most likely that this additional functionality will be provided by introducing a **text!** datatype that will act as being "character-based". The **string!** datatype will be retained and continue to act as being "code-point-based". Programmers will be able to switch between the two as they need.

The internal format for both string! and text! will be the same, the difference is the behaviours of how functions act on them. For example:

```
>> my-string: "^(0063)^(0327)"                                      ;; decomposed ç
== "ç"
>> head insert next my-string "1"
== "c1̧"

>> my-text: "^(0063)^(0327)"                                        ;; decomposed ç
== "ç"
>> head insert next my-text "1"
== "ç1"
```

## "Out of the Box" Tests
### The Tests
This is a brief set of tests to compare languages "out of the box" Unicode capabilities. Thanks to ["Musing Mortoray" blog article] (http://mortoray.com/2013/11/27/the-string-type-is-broken/) and the comments which provided a base for these tests. 

In this context, "out of the box" means capabilities either built-in to the language or its standard libraries that are supplied with the language. (I.E. No additional downloads).
```
1.  Equality of precomposed and decomposed characters
    Compare U+00E7 with "c" followed by U+0327
    The expected result is true.

2.  Non-equality of precomposed and decomposed characters  
    Compare U+00E7 with "c" followed by U+0327
    The expected result is false.

    (This test is to see if the language provides flexibility)

3.  Correct length of text containing decomposed characters
    Length of "noeU+0308l"
    The expected result is 4.

4.  Reversing a string containing decomposed characters
    Reverse "noeU+0308l"
    The expected result is "leU+0308on"

5.  Correct substring of a string containing decomposed characters
    Extract the first three characters of "noeU+0308l"
    The expected result is "noeU+0308"

6.  Correct uppercase of U+FB04
    Upper case of "baU+FB04e"
    The expected result is "BAFFLE"
    The length of the expected result is 6

7.  Correct uppercase of precomposed chars
    Upper case of "cantU+00F9"
    The expected result is "CANTU+00D9"

8.  Correct uppercase of decomposed chars
    Upper case of "cantuU+0300"
    The expected result is "CANTUU+0300"

9.  Processing above BMP
    Change treble clef symbol of "U+1D11E - The Treble Clef" to bass clef symbol(U+1D122)
    Change "Treble" to "Bass"
    The expected result is "U+1D122 - The Bass Clef"

10. Special Case - Turkish - Upper case "i"
    Set locale/Language to indicate Turkish 
    Upper case "i"
    The expected result is U+0130

    (Requires the ability to indicate that Turkish languages rules should be used.)

11. Special Case - Turkish - Lower case "I"
    Set locale/Language to indicate Turkish 
    Lower case "I"
    The expected result is U+0131

    (Requires the ability to indicate that Turkish languages rules should be used.)
   
12. Upper Case sharp s (U+00DF)
    Upper case "U+00DF"
    The expected result is U+00DF
```