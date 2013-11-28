## Introduction
There are a number of issues in using Unicode text encoding. This page gives a brief overview of the issues and the proposed resolution in Red.

## Encoding Normalisation
Many printable characters can be encoded in more than one way in Unicode. They may be encoded in a single code point (generally called a precomposed character) or a sequence of code points (decomposed) For example, whilst in ISO 8859-1 the letter 'ç' can only be represented as the single character E7 'ç', but in a Unicode encoding it can be represented as the single code point U+00E7 'ç' or the sequence U+0063 'c' U+0327 '¸'. A fuller understanding of the issue may be gained by reading this [Unicode.org technical report](http://unicode.org/reports/tr15/) and this [W3C paper](http://www.w3.org/TR/charmod-norm/).

Clearly for comparisons and sorting, Unicode encoded text must use a consistent encoding for each printable character. Unicode text can be "normalised" to use consistent encoding. There are in fact, four standard normalisations; two based on precomposed characters and two based on decomposed characters.

This is a real, not a theoretical, problem. Unicode text originating from Linux systems generally conforms to a precomposed normalisation whilst text originating from OS X systems conforms to a slightly non-standard decomposed normalisation.

### Encoding Normalisation Proposal
The choice of whether to normalise Unicode text and the method of normalisation is left to the user programmer. Built-in functions will be provided to normalise Unicode strings to one of the four standard normalisations.

  