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

##One Character, Multiple Code Points 
It is easy to fall into the trap that a Unicode Code Point is the equivalent of a printable character. It is clearly not as demonstrated by the example in the Normalisation section. For example, the length? function returns the number of code points in a string not the number of characters. Reversing strings containing decomposed characters will produce incorrect results. The string "ç", if encoded by the sequence U+0063 U+0327 when reversed would become "¸c".

###One Character, Multiple Code Points Proposal.
It is not felt worth the extra processing to change the default behaviour from treating code points individually. However, refinements will be provided that will take account of multi-code point characters.

For example: 

```
>>reverse "abçde"
=="ed¸cba"

>>reverse/decomp "abçde"
=="edçba"


  