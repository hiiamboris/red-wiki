# REPL (Console) Tips'n Tricks

Here you can find useful tips and functions you might want to use in REPL (Red's CLI or GUI console). Please add yours if you have one.

## Start-up script

You can create a shortcut to `red.exe` on your taskbar or start menu with below parameters to execute a startup script.

`<path>\red.exe --cli --catch user.red`

Or put this line into a batch file and execute it in your CLI.

Put `halt` to the end of your `user.red` file, with `--catch` parameter Red will keep the console open after executing the script.

## Tips

Define `>>` as `none` to be able to copy Red codes from web pages and paste them into console, no need to remove >> anymore:

`>>: none`


## Shell shortcuts

`q`, `ls`, `ll`, `pwd` are defined only in REPL and not when you compile your script.

## Useful Functions

You can put these handy functions to your `user.red` file.

### Clipboard functions

```
lc: load-clipboard: does [load read-clipboard]
dc: do-clipboard: does [do load-clipboard]
```

### what-is-my-ip

`what-is-my-ip: func [] [print read http://ident.me]`

### more (shell-like)

```
more: function [data [any-type!]][
	l: x: 0 d: copy []
	either file? data [d: read/lines (data)] [d: copy data]
	foreach x d [
		y: system/console/size/y
		print x  l: add l 1 if l >= y [l: 0 if "^[" =  ask "(more)" [break]]
	]
]
```

### cls

Clears CLI console screen

```
cls: does [
  unless system/console/gui? [
    call/console pick ["cls" "clear"] 'Windows = system/platform
  ]
  prin ""
]
```

### wait-key

```
wait-key: does [
  call/console pick ["pause" "read -rsp $'Press any key to continue...\n' -n1"] system/platform = 'Windows
]
```

### flatten

Flattens a block in any depth.

```
flatten: func [b [block!] /deep /local r value rule] [
    either deep [
        local: make block! length? b 
        rule: [
            into [some rule] 
            | set value skip (append local value)
        ] 
        parse b [some rule] 
        local
    ] [
        r: make block! length? b 
        head any [foreach v b [insert tail r v] r]
    ]
] 
```

```
>> flatten/deep [1 [2] [3 [4 [5 6]]] [7]]
== [1 2 3 4 5 6 7]
```

### explode

`explode: func [s] [parse s [collect any [keep skip]]]`

```
>> explode "abc"
== [#"a" #"b" #"c"]
```
