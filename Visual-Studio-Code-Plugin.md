Available here: https://github.com/red/VScode-extension

# Tips

## Running Red through Wine on Linux

Since there is no GUI support for Red on Linux yet, you might want to use the Windows version of Red through Wine.

You can configure the F6 / "Run Red script" command of the Red Visual Studio Code plugin to use the Windows version of Red through Wine by using a shell script:

```
#!/bin/sh
exec wine /replace/with/path/to/red-063.exe $1 "`winepath --windows \"$2\"`"
```

Save this as `winered.sh`, edit the script to point to `red-063.exe` on your computer, give it executable permissions with `chmod +x` and then in your user settings in Visual Studio Code, set `red.redPath` to `winered.sh`.