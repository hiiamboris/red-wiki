# REPL (Console) Tips'n Tricks

Here you can find useful tips and functions you might want to use in REPL (Red's CLI or GUI console). Please add yours if you have one.

## Executing the REPL

Don't use `red --cli` when you want to invoke the Red CLI console from another app. Just call directly the CLI console executable (in the Red cache folder, or compile one in the working folder). `red --cli` is just a handy shortcut when wanting to invoke the CLI console from the shell command-line.

Red.exe is a R2-encapped binary with a non-standard booting sequence that often does not play well when called by third-party apps. When Red.exe is used to invoke the console, it acts as a proxy by sub-calling the right console executable. 
Just avoid the overhead and possible trouble by calling the console exe directly in such cases.

## Start-up script

You can create a shortcut to `red.exe` on your taskbar or start menu with below parameters to execute a startup script.

`<path>\red.exe --cli --catch user.red`

Or put this line into a batch file and execute it in your CLI.

Put `halt` to the end of your `user.red` file, with `--catch` parameter Red will keep the console open after executing the script.

## Tips

Define `>>` as `none` to be able to copy Red codes from web pages and paste them into console, no need to remove >> anymore:

`>>: none`

## Prompt

You can customize the prompt in the console to a fixed string, or a function that returns a string:
```
system/console/prompt: does [append what-dir " >> "]
system/console/prompt: does [append form now " >> "]
```

## CLI console key bindings

Instead of keys like `Home` and `End` which might not work in some terminals or platforms common readline key bindings like `C-a` (Control+a) or `C-e` can be used to go to beginning or the end of input line.

More supported keys can be found [there](https://github.com/red/red/blob/master/environment/console/CLI/input.red#L45), most of them act like in any readline compatible app.

## Rlwrap as readline compatible interface for Red console

If key bindings provided by CLI console not enough then command like `rlwrap -i -a ~/.red/console*` (assuming that rlwrap installed in the system) will run console with readline library wrapper as a primary input interface providing more readline compatible key bindings. Now it have some output artefacts, but some of those issues may be solved with additional rlwrap configuration.

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
