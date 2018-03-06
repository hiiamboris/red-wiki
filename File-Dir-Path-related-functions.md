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

