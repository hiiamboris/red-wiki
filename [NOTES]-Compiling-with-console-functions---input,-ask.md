To compile a script that uses the `input` or `ask` functions, you will need:
* Download latest `red`

  * For Windows: https://static.red-lang.org/dl/auto/win/red-latest.exe
  * For Linux: https://static.red-lang.org/dl/auto/linux/red-latest
  * For macOS: https://static.red-lang.org/dl/auto/mac/red-latest
  
* Include the following line at the beginning of your script: 
`#include %environment/console/CLI/input.red`

* Compile in release mode using the -r flag

Examples:

For Linux:
`red -r -t Linux your-red-program.red`

For macOS:
`red -r -t Darwin your-red-program.red`

For Windows:
`red -r -t MSDOS your-red-program.red`

***

****Compilation table:****
```red
MSDOS        : Windows, x86, console (+ GUI) applications
Windows      : Windows, x86, GUI applications
WindowsXP    : Windows, x86, GUI applications, no touch API
Linux        : GNU/Linux, x86
Linux-ARM    : GNU/Linux, ARMv5, armel (soft-float)
RPi          : GNU/Linux, ARMv5, armhf (hard-float)
Darwin       : macOS Intel, console-only applications
macOS        : macOS Intel, applications bundles
Syllable     : Syllable OS, x86
FreeBSD      : FreeBSD, x86
Android      : Android, ARMv5
Android-x86  : Android, x86
```

# Clearing %libRedRT when using `-o` on the command line

Normally you can just do `rebol -qs red.r clear`, which clears %libRedRT in the working folder. If you use `-o` to specify an alternate output dir, e.g., `rebol -qs red.r -c -o ../../Code/Red/test ../../Code/Red/test.red`, you need to add a path argument to `red clear`, because %libRedRT is built in the same dir as the executable. Otherwise the exe couldn't find it.
