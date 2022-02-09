# XML codec

See also https://gitlab.com/rebolek/markup/-/wikis/home for implementation details and discussions.

XML codec is used to read XML data and convert them to the Red format and vice versa.
Because of the nature of the XML that does not map very well to existing Red data structures,
we decided to make it modular and support multiple output and input formats.

## Formats

There are currently three formats supported: `triples`, `compact` and `key-val`. More formats may
be added in the future when we find more optimal representation of the XML data.
The default format is `triples`, to use different format, either use `/as` refinement with
the format name as a `word!`.

All examples are using this XML source:

```
<?xml version="1.0" encoding="UTF-8" ?>
    <identity>
        Version and language
        <version number="$Revision$"/>
        <language type="root">Czech</language>
        <markup />
        Ending
    </identity>
```

### Triples

`triples` is the default format. Its advantage is a fixed structure that can be explored
easily using functions like `foreach` and HOFs.

Each XML element is stored using three values:

```
tag-name content attributes
```

The order is optimized for `path!` access, so it’s possible to get the tag’s content easily.

`tag-name` is `word!` but it can be overriden and tag names can be stored as `string!`
when case-sensitivity is required (it shouldn’t be in most cases). To turn the conversion off,
use `/str-keys` refinement. For the textual data, `tag-name` is `text!` (see examples below).

`content` is either the textual content, a `block!` of sub-elements or `none!` if there’s no content.

`attributes` is a `block!` containing key-value pairs of attributes or `none!` if there are no attributes.

output:

```
[
    identity [                                  ; content is a block! here
        text! "Version and language^/" none     ; textual data, tag-name is none, no attributes either
        version none [number "$Revision$"]      ; no content, so it’s none
        language "Czech" [type "root"]          ; tag with text content and attributes
        markup none none                        ; tag without content and attributes  
        text! "Ending^/" none                   ; textual data, see above
    ] none                                      ; (empty) attributes for the <identity> tag
]
```

### Compact

`compact` format optimizes storage at the price of a more difficult programatic access. Elements always start with
the tag name as `word!` followed by a `block!` which contains the content and attributes. Text content is prefixed
with `text!`, attributes are stored as key-value pairs, key is always `issue!` and value is a `string!`. See example below: 

output:

```
[
    identity [
        text! "Version and language^/"          ; textual data, prefixed with `text!`
        version [#number "$Revision$"]          ; tag without text with one attribute
        language [#type "root" text! "Czech"]   ; tag with text and attributes
        markup []                               ; empty tag is followed by empty block, so the path access still works
        text! "Ending^/"                        ; textual data, see above
    ]
]
```

### Key-val

`key-val` is similar to `triples` but rather than relying on position, it uses `word!` to signify the structure.
The are two keywords: `text!` for textual content of `string!` type and `attr!` for attributes (`block!`).
If there’s no text or attributes, the value is omitted.

output:

```
[ 
    identity [
        text! "Version and language^/"               ; textual data, prefixed with `text!`
        version [attr! [number "$Revision$"]]        ; tag without text with one attribute
        language [text! "Czech" attr! [type "root"]] ; tag with text and attributes
        markup []                                    ; empty tag
        text! "Ending^/"                             ; textual data, see above
    ]
]
```

## Commands

### LOAD-XML

```
USAGE:
    LOAD-XML data

DESCRIPTION:
    Convert XML data to Red format.
    LOAD-XML is a function! value.

ARGUMENTS:
    data         [string! file! url!] "XML to convert."

REFINEMENTS:
    /as          =>
       fmt          [word!] "Select output format [triples compact key-val]."
    /meta        => Preserve meta data.
    /str-keys    => Leave keys as strings.
```

* `/as` is used to change the output format. Default format is `triples`.

* `/meta` is used to preserve document metadata, such as XMLdecl, doctype, comments, processing instructions and cdata. These data are normally omitted from the resulting document.

* `/str-keys` is used to leave keys as strings. Normally, they are converted to words for easier access but some keys may be incompatible with `word!` syntax so there’s an option to skip the conversion.

### TO-XML

```
USAGE:
    TO-XML data

DESCRIPTION:
    TO-XML is a function! value.

ARGUMENTS:
    data         [block!] "Red data for conversion."

REFINEMENTS:
    /as          =>
       fmt          [word!] {Format of the source data [triples compact key-val].}
    /pretty      =>
       indent       [string!] {Pretty format the output, using given indentation.}
```

* `/as` is used to change the input format. Default format is `triples`.

* `/pretty` is used to switch from compact format to a pretty one. **NOTE: This option works with `triples` mode currently only!**
    `/pretty` requires `indent` parameter which is a `string!`that will be used as the basic indentation. Suggested values are spaces or a tab.

## Metadata

If you convert a source XML to Red format and want to filter out just metada, you can use `get-meta` function from [%xml-tools.red](https://gitlab.com/rebolek/markup/-/blob/main/xml-tools.red).

```
USAGE:
     GET-META data

DESCRIPTION: 
     Get metadata only from converted XML data. 
     GET-META is a function! value.

ARGUMENTS:
     data         [block!] "Converted XML data."

REFINEMENTS:
     /as          => Select output format [triples compact key-val].
        format       [word!] 
```
