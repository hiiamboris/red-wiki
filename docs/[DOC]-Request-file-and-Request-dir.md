## Request-file
```red
request-file  /save  /multi  /file  name  /title  text  /filter  list

  Asks user to select a file and returns full file path (or block of paths).

Refinements:
  /save  - File save mode
  /multi - Allows multiple file selection, returned as a block
  /file
      name [file!] - Default file name or directory
  /title
      text [string!] - Window title
  /filter
      list [block!] - Block of filters (filter-name filter) 
```

### Filtering file views
You can provide file list filters to show only specific files. 

```red
[
    description-1 file-extensions-1
    description-2 file-extensions-2
    ......
]
```
* description: (**string!**) Describes the filter (for example, "`Text Files`")
* file-extensions: (**string!**) Specifies the filter pattern (for example, "`*.TXT`"). To specify multiple filter patterns for a single display string, use a semicolon to separate the patterns (for example, "`*.TXT;*.DOC;*.BAK`")

For example:

```red
request-file/filter [
    "Red Source Files (red, reds)" "*.red;*.reds"
    "All Pictures Files" "*.jpg;*.png;*.gif"
]
```

### Notes

Remember that when setting windows title,
* ^ has special meaning in Rebol/Red string, you need to type ^^ to insert a caret.
* & is used for underline the character on Windows. Type && for a single & character. Other platforms no such feature.

[See #2610](https://github.com/red/red/issues/2610)