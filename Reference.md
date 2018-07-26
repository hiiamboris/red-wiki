External links appear in quotes.
***

* ["Red Language Documentation"](http://www.red-lang.org/p/documentation.html)

* [VID Reference Documentation](https://github.com/red/red/wiki/%5BDOC%5D-VID-Reference-Documentation)

* ["Red/System Documentation"](http://static.red-lang.org/red-system-specs-light.html)

* ["Red/System - 语言规范-中文翻译"](https://github.com/red/red/wiki/%5Bzh-hans%5D-Red-System-Language-Specification-Chinese-Traslation)

* ["Coding Style Guide"](https://github.com/red/red/wiki/%5BDOC%5D-Coding-Style-Guide)

* [Contributors Guidelines](https://github.com/red/red/wiki/%5BDOC%5D-Contributor-Guidelines)

* [Conversions to string](https://github.com/gltewalt/red/wiki/%5BDOC%5D-Conversions-to-string)

* [Using `call`](https://github.com/gltewalt/red/wiki/%5BDOC%5D-Reference-Call)

* [The Handling of NaNs, INFs, and signed zeros](https://github.com/red/red/wiki/%5BDOC%5D-The-Handling-of-NaNs,-INFs-and-signed-zeros.)

* [Red Enhancement Proposals](https://github.com/red/red/wiki/%5BDOC%5D-REP---Red-Enhancement-Proposals)

***

## Using Red on Linux with Wine

Since there is no GUI support yet for the Linux version of Red, you might want to run the Windows version of Red with Wine. It works remarkably well without any extra configuration, but DRAW commands are a bit slow out of the box.

You can achieve a x10 speed increase by installing `gdiplus` with ["Winetricks"](https://wiki.winehq.org/Winetricks), which is a simple bash script that can be installed through the package manager of most Linux distributions, or just downloaded from its website.

You can then install `gdiplus` by opening a terminal and entering: `winetricks gdiplus`