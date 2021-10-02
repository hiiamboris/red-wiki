# Editor Integrations

List of IDE / Text Editor integrations
TODO: add feature table (syntax highlight, autocomplete, inline docs; see also rust racer)

## External
That is, not written in red

### VS-code
Install from the [Marketplace](https://marketplace.visualstudio.com/items?itemName=red-auto.red), or from VS Code itself: `ctrl+shift+P`, `> ext install red-auto.red`

[Sources](https://github.com/red/VScode-extension)

### Sublime-text
[Red](https://packagecontrol.io/packages/Red) - Basic syntax highlighting.
Install with `ctrl+shift+P` (windows/linux) or `cmd+shift+P` (mac) and type "Install Package" and hit `enter`, then wait for the package index to load and look for "Red". If you don't already have Package Control installed, as of build 3143 you can install it from the command palette (`ctrl+shift+P`) with "Install Package Control".


### for Sublime Text 3 users (Build System): 

```red
{
    "shell_cmd": "/Users/apple/Desktop/
    /red-063.dms -c \"${file}\" && \"${file_path}/${file_base_name}\""
}
```

this will compile the Red file that is on your screen and execute it

### [Howl](https://howl.io/)
[rokf/howl-red](https://github.com/rokf/howl-red)

### [EditPadPro](https://www.editpadpro.com/)
[Syntax Coloring](https://www.editpadpro.com/cgi-bin/cscsdl4.pl?id=282)

### Emacs
[Emacs mode for Red](https://github.com/unchartedworks/red-mode)

### Vim
[Red language syntax highlighting](https://github.com/ruchee/vim-red)

### Notepad++
[Syntax Coloring](https://github.com/Ungaretti/Notepad-config-file-for-Red-Language)

[Makeshift IDE for RED Language development](https://github.com/Ungaretti/Makeshift-IDE-for-Red-language) - Instructions for making a portable Red IDE out of Notepad++ by [Ungaretti](https://ungaretti.github.io/)

[Rededitor](http://helpin.red/Rededitor.html) - A pre-made portable Red IDE using Notepad++ by [Ungaretti](https://ungaretti.github.io/) similar to the makeshift IDE he explains how to make above

### [Editplus](https://editplus.com)
Download syntax coloring, auto-complete and cliptext files from this [page](https://editplus.com/others.html) by searching for "Red programming language" or directly from [here](https://editplus.com/dn.php?n=red.zip).

Extract and copy all files to `%APPDATA%\Editplus` then open `Tools / Preferences / Settings & Syntax` add Red to file types, enter `red;reds` to file extension, select syntax (.stx) and auto-complete (.acp) files, configure colors then apply.

### CudaText

[CudaText wiki](https://wiki.freepascal.org/CudaText_plugins#LSP_server_for_Red)
[loziniak/cudatext-red](https://github.com/loziniak/cudatext-red)

## Red Native

Editors written in Red itself

## Writing your own

### Useful words
`help-string`, `red-complete-input`

## LaTeX
To produce PDF files with programming code syntax coloring there are at least two packages:

* [minted](https://ctan.org/pkg/minted), supports Red and REBOL. Requires Python and Pygments.
* [listings](https://ctan.org/pkg/listings), does not (yet) support Red.