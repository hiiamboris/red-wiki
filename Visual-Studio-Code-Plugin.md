Available here: https://github.com/red/VScode-extension

# Tips

## Running Red through Wine on Linux

Since ~~there is no GUI support for Red on Linux yet~~ ([now there is GUI support in Linux](https://github.com/red/red/commit/8cc711616f64d7e9762c76a284bc559a730a052a)), you might want to use the Windows version of Red through Wine.

You can configure the F6 / "Run Red script" command of the Red Visual Studio Code plugin to use the Windows version of Red through Wine by using a shell script:

```red
#!/bin/sh
exec wine /replace/with/path/to/red-063.exe $1 "`winepath --windows \"$2\"`"
```

Save this as `winered.sh`, edit the script to point to `red-063.exe` on your computer, give it executable permissions with `chmod +x` and then in your user settings in Visual Studio Code, set `red.redPath` to `winered.sh`.

## Speed up DRAW commands through Wine

You can achieve a x10 speed increase by installing `gdiplus` with ["Winetricks"](https://wiki.winehq.org/Winetricks), which is a simple bash script that can be installed through the package manager of most Linux distributions, or just downloaded from its website.

You can then install `gdiplus` by opening a terminal and entering: `winetricks gdiplus`