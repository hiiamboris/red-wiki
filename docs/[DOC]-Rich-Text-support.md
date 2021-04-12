This is our plan for supporting rich text features in Red/View. 

What we call "rich text" here is one or several paragraphs of text, where different graphic styles can be applied to segments of the text. The proposed API has three different levels, from simplest to most optimized:

1. Using the RTD dialect (from VID, or when manually constructing the face).
2. A low-level rich styling dialect for one text string (for fast performance).
3. Multiple rich text paragraphs in a single face (for complex layouts).

## High-level Rich-Text Dialect

This Rich-Text Dialect (RTD) provides a high-level interface for specifying styled text. Styles application range can be specified using starting/ending delimiters or using blocks.

**Grammar**

`any-single` is a pseudo-keyword meaning: *any combination of single occurence of following alternatives*.
```red
color-name: word!
nested: [ahead block! into rtd]
color:  [tuple! | issue! | color-name]

f-args: [
    ahead block! into [integer! string! | string! integer!]
    | integer!
    | string!
]
style: [
      ['b | 'bold      | <b>]           [nested | rtd [/b | /bold      | </b>]]
    | ['i | 'italic    | <i>]           [nested | rtd [/i | /italic    | </i>]]
    | ['u | 'underline | <u>]           [nested | rtd [/u | /underline | </u>]]
    | ['s | 'strike    | <s>]           [nested | rtd [/s | /strike    | </s>]]
    | ['f | 'font      | <font>] f-args [nested | rtd [/f | /font      | </font>]]
    | ['bg | <bg>] color                [nested | rtd [/bg             | </bg>]]
    | tuple!        opt nested          ;-- color as R.G.B tuple
    | issue!        opt nested          ;-- color as #rgb or #rrggbb hex value
    | color-name    opt nested
    | ahead path!
      into [any-single ['b | 'i | 'u | 's | color-name]]
      nested
]
rtd: [some [style | string!]]
```
Notes:

* The path syntax allows to combine several styles together to be applied on the block following the path.
* Mixed usage of delimiters and blocks for different styles is allowed.

**Usage**

RTD input is processed by a specific `rtd-layout` function that will return a single-box rich-text face, where the RTD code will be compiled to a single text string (stored in `/text` facet) and a low-level styling description (stored in `/data` facet). The full specification of the function is:
```red
rtd-layout: func [rtd [block!] /with target [face!] return: [face!]]
```

**Examples**
```red
rtd-layout [<i> <b> "Hello" </b> <font> 24 red " Red " </font> blue "World!" </i>]

rtd-layout [i b "Hello" /b font 24 red " Red " /font blue "World!" /i]

rtd-layout [i [b ["Hello"] red font 24 [" Red "] blue "World!"]]

rtd-layout [i/b/u/red ["Hello" font 32 " Red " /font blue "World!"]]
```

## Low-level Styling Dialect

This dialect describes a list of styles to be applied on the string referred by `/text` facet in a rich-text face. The purpose of this dialect is to provide a solution for dynamic changes and info querying that performs as fast as possible. This also maps well with the underlying hardware-accelerated APIs (it relies on Direct2D on Windows).

The dialect grammar is a simple list of text segments (defined using a starting position and a length combined in a  `pair!` value) followed by a list of styles. So, the typical structure is:
```red
[
    <range1> style1 style2 ...        ;-- range1: start1 x length1
    <range2> style1 style2 ...        ;-- range2: start2 x length2
    ...
]
```
Styles can overlap, and later styles have higher priority (cascading styles).

The following styles are supported:
```red
[
      tuple!                                  ;-- text color
    | backdrop tuple!                         ;-- background color
    | bold                                    ;-- bold font
    | italic
    | underline tuple! (color)  lit-word! ('dash, 'double, 'triple)    ;@@ color and type are not supported yet
    | strike tuple! (color) lit-word! ('wave, 'double)                 ;@@ color and type are not supported yet
    | border tuple! (color) lit-word! ('dash, 'wave)                   ;@@ not implemented
    | integer!                                ;-- new font size
    | string!                                 ;-- new font name
]
```

## Rich text face type

A new native `rich-text` face type supports rich text features with underlying hardware-acceleration. The face has two modes for displaying rich text:

### Single-box mode

The whole face area is used for displaying the rich text, starting at upper left corner, using the following specific facets:

* `/data` (block!): a block of low-level styling dialect instructions to be applied on `text` facet.
* `/text` (string!): a text string to be displayed using the `/data` facet styles description.

Draw facet can still be used and it will be rendered on top of the rich text display.

**Example**
```
view [rich-text "hello" with [data: [2x3 bold]]]
```

### Multi-box mode

In this mode, an arbitrary number of rich text areas can be displayed inside the same rich-text face. In order to achieve that, each rich text area is specified using the `text` keyword in Draw dialect.

Specific facets:
* `/draw` (block!): a block of `text` instructions, eventually mixed with regular Draw instructions.
* `/text` (none!): this facet must be set to `none` in order to enable this mode.

**Draw extension**
```
text <pos> <text>

<pos>  : a pair! value indicating the upper left corner of the text-box.
<text> : a string, or a rich-text face object with a rich-text description in single-box mode.
```

This mode is optimized for lowest system resources usage when a high number of rich text areas need to be rendered.


## Info querying functions

The following functions are provided to query information about a rich-text face content. Those functions can be used to easily implement:

* cursor navigation
* hit testing

From global context:
```
caret-to-offset: function [
    "Given a text position, returns the corresponding coordinate relative to the top-left of the layout box"
    face    [object!]
    pos     [integer!]
    return: [pair!]
]

offset-to-caret: function [
    "Given a coordinate, returns the corresponding text position"
    face    [object!]
    pt      [pair!]
    return: [integer!]
]
    
size-text: function [
    "Returns the area size of the text in a face" 
    face [object!]
    /with                   ;-- unused for rich-text
        text [string!]
    return: [pair! none!]
]
```
From `rich-text` context:
```
line-height?: function [
    "Given a text position, returns the corresponding line's height"
    face    [object!]
    pos     [integer!]
    return: [integer!]
]

line-count?: function [
    "number of lines (> 1 if line wrapped)"
    face    [object!]
    return: [integer!]
]
```