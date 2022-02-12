NOTE: It's possible for this to get out of date with current Red functions. To see what's available in your current version of Red, use `help`. e.g.

```red
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
| `glob`            | [Recursively list files/directories matching patterns (currently unofficial).](https://gist.github.com/hiiamboris/605a4ab6831a247ac987789f4d578ef1)| 
| `query`           | Returns the file creation date |

# Call
[Full reference here](https://github.com/red/red/wiki/Reference-Call)

# Clean-path

# Normalize-dir

# Split-path

# File! vs path! values

`File!` values are a string type. There are some funcs that work on them, e.g. `split-path` that "know" about path separators, but the datatype itself doesn't. `Path!` values are block values, where each slot can contain a different type of Red value, though not every value type, and most commonly words. One thing they can't contain is a `set-word!` value in any slot other than the last value. If there is a `set-word!` in the first slot, they are currently lexed as a `url!`, which is a different question, but may help explain why you can't treat these types interoperably or convert between them with complete confidence. It may be tempting to use path! values when trying to make file values, but likely won't be the best approach.

# How do I ...?

## Define a file as a string

You can use a literal string, or use Red notation for a filename.

```red
>> path: "C:\windows\"
== "C:\windows\"
>> path: %/c/windows/
== %/c/windows/
```

## Make a block of file path components from a file path

For a local filename string
```red
>> path: "C:\windows\"
== "C:\windows\"
>> split path #"\"
== ["C:" "windows"]
```

For a Red file!
```red
>> path: %/c/windows/
== %/c/windows/
>> split-path path
== [%/c/ %windows/]
```

## Make a Red compatible filename
```red
>> path: "C:\windows\"
== "C:\windows\"
>> to-red-file path
== %/C/windows/
```

## Make a local OS compatible filename

```red
>> path: %/c/windows/
== %/c/windows/
>> to-local-file path
== "c:\windows\"
```

## Get a block of fully qualified directories

As always, there are many ways to do things in Red. We'll just provide a couple examples.

```red
>> collect [foreach file read to-red-file path [if dir? file [keep clean-path file]]]
== [%/Z/home/isheh/dev/red/logs/ %/Z/home/isheh/dev/red/inf/ %/Z/home/isheh/dev/red/mono/ %/Z/home...
```
or

```red
>> files: read path
== [%addins/ %AppCompat/ %AppPatch/ %ARJ.PIF %assembly/ %atiogl.xml %ativpsrm.bin %atl80.dll %bfsvc.exe %Boot/ %boots...
>> remove-each file files [not dir? file]
>> files
== [%addins/ %AppCompat/ %AppPatch/ %assembly/ %Boot/ %Branding/ %CSC/ %Cursors/ %debug/ %diagnostics/ %DigitalLocker...
>> forall files [files/1: clean-path files/1]
== %/D/<redacted>/build/bin/winsxs/
>> files
== [%/D/<redacted>/build/bin/addins/ %/D/<redacted>/build/bin/AppCompat/ ...
>> 
```

## Get a block of unqualified contents (file or dir) in a directory

```red
>> read to-red-file path
== [%logs/ %inf/ %mono/ %winsxs/ %syswow64/ %twain_32.dll %win.ini %Installer/ %Fonts/ %twain.dll ...
```

## Get a block of filenames, no dirs, for a directory
```red
>> collect [foreach file read to-red-file path [unless dir? file [keep file]]]
== [%twain_32.dll %win.ini %twain.dll %winhlp32.exe %system.ini %regedit.exe %explorer.exe %notepa...
```

## Test if a directory or file exists
Directory test:
```red
>> dir-exists?: func [f [file!]] [exists? dirize f]
>> dir-exists? %./
== true
>> mkdir %dir  dir-exists? %dir
== true
```
File test:
```red
>> file-exists?: func [f [file!]] [all [exists? f not exists? dirize f]]
>> file-exists? %.
== none
>> write %file ""  file-exists? %file
== true
```

# Directory junctions & symbolic links (Windows-specific)
Some of the directories returned in a `read` call might not themselves be readable:
```red
>> read to-red-file "C:\Documents and Settings\"
*** Access Error: cannot open: %/C/Documents%20and%20Settings/

>> call/console {dir "C:\Documents and Settings"}
<...>
File not found
```

We can see that the above is a junction and it's target *is* readable:

```red
>> call/console {dir /a "C:\Docu*"}
<...>
14.07.2009  08:08    <JUNCTION>     Documents and Settings [C:\Users]

>> read to-red-file "C:\Users\"
== [%All%20Users/ %Default/ %Default%20User/ %desktop.ini %Public/]

>> read/binary %/C/Documents%20and%20Settings/desktop.ini
== #{
FFFE0D000A005B002E005300680065006C006C0043006C006100730073004900
6E0066006F005D000D000A004...
```

This happens because the user does not have the permissions to list the link/junction contents (RD stands for Read Data, which is Denied), but have permissions to read the target:

```red
>> call/console {icacls "C:\Documents and Settings"}
C:\Documents and Settings All:(DENY)(S,RD)
<...>

>> call/console {icacls "C:\Users"}
C:\Users All:(RX)
         All:(OI)(CI)(IO)(GR,GE)
<...>
```

So as you can see the OS has different ACLs assigned to the link itself and its' target. Apparently the junction is there for compatibility for WinXP and earlier and isn't supposed to be used. The reader is advised to learn about file system access control lists and commands `icacls` and `cacls` to get a more thorough technical outlook.

As to "omg! what to do?", the options depend on the specifics of the task at hand. Once `query` function is implemented it will be possible to:
- ignore all files with hidden or system attributes set (or both)
- resolve the target of a link and try to query target's contents instead

In any case, it's always a good idea to wrap the `read` call into a `try` block, as one can never know if one is allowed to read a target or not:

```red
>> probe try [read to-red-file "C:\$Recycle.Bin"] ()
make error! [
    code: 500
    type: 'access
    id: 'cannot-open
    arg1: %/C/$Recycle.Bin
    arg2: none
    arg3: none
    near: none
    where: 'read
    stack: 37521760
]
```

# UNC support
Watch for https://github.com/red/red/issues/3422 progress

