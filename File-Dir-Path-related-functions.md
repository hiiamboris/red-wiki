NOTE: It's possible for this to get out of date with current Red functions. To see what's available in your current version of Red, use `help`. e.g.

```
help file
help path
help dir
```

Red contains a number of standard functions related to file and directory paths. If you're coming from another language, the names may differ from what you're used to, or the functionality may not be exactly the same. Some exist for Rebol compatibility, but also had to be considered *worth* including. Others are new, and still may be open for design changes. More are yet to come, surely. This document can be used to see how they work, individually, and together, for both learning and discussions on how to provide the best set of file functions possible.

Note that `file!` values in Red are file *names*, not file *contents*. Access to file contents will come with ports. For example `length?` refers to the length of the filename, while `size?` refers to the file contents.

| Function          | Notes |
|-------------------|---------------------------------------------------------------------------------------|
| `call`            | [See below](#call) | 
| `cd`              | Shortcut alias for `change-dir`. Handy in the console.| 
| `change-dir`      | Changes the active directory path, relative or absolute, using a Red file path.| 
| `clean-path`      | [See below](#clean-path) | 
| `create-dir`      | OS level routine to create a directory in the file system. Use `make-dir` instead.| 
| `dir`             | Shortcut alias for `list-dir`. Handy in the console. Arg optional.| 
| `dir?`            | Returns true file value ends with #"/". Does not interact with file system.| 
| `dirize`          | Add #"/" to the end of the value if not already there. Does not interact with file system.| 
| `exists?`         | Returns true file file or dir exists in the file system. Uses Red file path.| 
| `file?`           | Returns true if Red value is a `file!` type. Does not interact with file system.| 
| `get-current-dir` | Return current dir, as an OS-local filename. Not normalized.| 
| `list-dir`        | List files and directories from given folder. Uses Red file path. Arg required.| 
| `ll`              | Shortcut alias for `list-dir` (single column). Handy in the console. Arg optional.| 
| `ls`              | Shortcut alias for `list-dir`. Handy in the console. Arg optional.| 
| `make-dir`        | Wrapper for `create-dir`. Can create deep dirs in one call.| 
| `normalize-dir`   | [See below](#normalize-dir) | 
| `request-dir`     | User interactive dir requestor.| 
| `request-file`    | User interactive file requestor.| 
| `set-current-dir` | Changes the active directory path, relative or absolute, using a local system file path.| 
| `split-path`      | [See below](#split-path) | 
| `to-file`         | Coerce a Red value to a `file!` value. No other changes.| 
| `to-local-file`   | Converts a Red file path to the local system file path.| 
| `to-red-file`     | Converts a local system file path to a Red file path.| 
| `what-dir`        | Return current dir, as a Red file path. Normalized to have a trailing slash.| 

# Call

# Clean-path

# Normalize-dir

# Split-path

# File! vs path! values

`File!` values are a string type. There are some funcs that work on them, e.g. `split-path` that "know" about path separators, but the datatype itself doesn't. `Path!` values are block values, where each slot can contain a different type of Red value, though not every value type, and most commonly words. One thing they can't contain is a `set-word!` value in any slot other than the last value. If there is a `set-word!` in the first slot, they are currently lexed as a `url!`, which is a different question, but may help explain why you can't treat these types interoperably or convert between them with complete confidence. It may be tempting to use path! values when trying to make file values, but likely won't be the best approach.

# How do I ...?

## Define a file as a string

You can use a literal string, or use Red notation for a filename.
```
>> path: "C:\windows\"
== "C:\windows\"
>> path: %/c/windows/
== %/c/windows/
```

## Make a block of file path components from a file path

For a local filename string
```
>> path: "C:\windows\"
== "C:\windows\"
>> split path #"\"
== ["C:" "windows"]
```

For a Red file!
```
>> path: %/c/windows/
== %/c/windows/
>> split-path path
== [%/c/ %windows/]
```

## Make a Red compatible filename
```
>> path: "C:\windows\"
== "C:\windows\"
>> to-red-file path
== %/C/windows/
```

## Make a local OS compatible filename
```
>> path: %/c/windows/
== %/c/windows/
>> to-local-file path
== "c:\windows\"
```

## Get a block of fully qualified directories
```
>> collect [foreach file read to-red-file path [if dir? file [keep clean-path file]]]
== [%/Z/home/isheh/dev/red/logs/ %/Z/home/isheh/dev/red/inf/ %/Z/home/isheh/dev/red/mono/ %/Z/home...
```

## Get a block of unqualified contents (file or dir) in a directory
```
>> read to-red-file path
== [%logs/ %inf/ %mono/ %winsxs/ %syswow64/ %twain_32.dll %win.ini %Installer/ %Fonts/ %twain.dll ...
```

## Get a block of filenames, no dirs, for a directory
```
>> collect [foreach file read to-red-file path [unless dir? file [keep file]]]
== [%twain_32.dll %win.ini %twain.dll %winhlp32.exe %system.ini %regedit.exe %explorer.exe %notepa...
```
