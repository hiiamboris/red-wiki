# [DOC] Reference - CALL function

The CALL function enables RED programs to execute and communicate with the system's shell interface.

Any built-in shell command or application can be launched by CALL.

Shell commands depend on the operating system.

Input and output redirection allows RED to exchange data with shell command.

---

The RED CALL function is slightly different from the REBOL CALL. Read carefully this document to discover differences.

As RED 0.41 doesn't implement yet types as *url!* or *port!*, and the *file!* type is not fully supported, the call function redirection apply on *string!* or *block!*.

The CALL function is not provided with the default RED implementation.
Add `#include %system/library/call/call.red` to your source code to include the feature
or compile the console-call example from the Red directory :

*	`$ red -c tests/console-call.red`	Unix, MacOSX
*	`C:\Red>red.exe -c tests\console-call.red`	Windows

Linux and MacOSX users run **console-call** from command line.  Windows users run **console-call.exe**.

## Getting help
```red
	red>> help call

	USAGE:
		call cmd /wait /console /shell /input in /output out /error err

	DESCRIPTION:
		Executes a shell command to run another process..
		call is type: function!

	ARGUMENTS:
		cmd [string! block!] => A shell command, an executable file or a block.

	REFINEMENTS:
		/wait => Runs command and waits for exit.
		/console => Runs command with I/O redirected to console.
		/shell => Forces command to be run from shell.
		/input
			in [string! block!] => Redirects in to stdin.
		/output
			out [string! block!] => Redirects stdout to out.
		/error
			err [string! block!] => Redirects stderr to err.
```

## Call syntax

Default behavior is asynchronous, CALL returns the child's PID (Process ID). No output is printed.

### Basic call

	call cmd

The CALL function accepts one argument, a *string!* or a *block!*. Blocks are formed to a string before being passed as argument.

These examples passes the Unix copy command **cp** with two arguments.

	call "cp source.red dest.red"			; string! command
	call [ "cp" "source.red" "dest.red" ]	; block! command

If a block contains words, it must be reduced first.

	s: "source.red"
	d: "dest.red"
	call reduce [ "cp" s d ]			; block! command

As the *file!* type is not yet implemented. Character "/" used into path values are not translated from RED to windows.

	call "ls mydir/*"		; Unix, OSX

	call "dir mydir\*"		; Windows

The CALL function can launch any GUI application :

	call "explorer"			; Windows, starts windows's explorer

	call {"C:\Program Files\Mozilla Firefox\firefox.exe"}		; Windows, start firefox with full path

	call "firefox"			; Unix, start firefox if found in your PATH

### Refinements

#### **/wait**

Cause CALL to wait for child process is completed. Return -1 if there is an execution error or child process exit code.

```red
	red>> call "cp source.red dest.red"
	== 7227					; PID
	red>> call/wait "cp source.red dest.red"
	== 0					; 0 = process complete or error code
	red>>

/input, /output or /error refinements implicitly set the /wait refinement.
```

#### **/console**

Cause the process' stdout and stderr to be printed to the console.

With no **/wait** refinement, the process ID is shown first and RED returns to prompt before the command output is printed.
```red
	red>> call/console "ls"
	== 7946
	red>> boot.red     build         console-call      lexer.r       README.md  red.r      tests
	bridges            call-example  console-call.bin  lexer.red     Red        run-all.r  usage.txt
	BSD-3-License.txt  compiler.r    console-call.exe  pdcurses.dll  red.bin    runtime    utils
	BSL-License.txt    console       docs              quick-test    red.exe    system     version.r
```
With **/wait** refinement, the CALL returned value is printed after the command output ended.
```red
	red>> call/console/wait "ls"
	boot.red           build         console-call      lexer.r       README.md  red.r      tests
	bridges            call-example  console-call.bin  lexer.red     Red        run-all.r  usage.txt
	BSD-3-License.txt  compiler.r    console-call.exe  pdcurses.dll  red.bin    runtime    utils
	BSL-License.txt    console       docs              quick-test    red.exe    system     version.r
	== 0
	red>>
```
#### **/input**

A string or a block can be redirected as input to the launched process.

String examples :
```red
	red>> call/input "cat" {This is a Red world^/}
	== 0
	red>> call/input/console "cat" {This is a Red world^/}
	This is a Red world
	== 0
	red>> data: {white^/red^/green^/blue^/magenta^/cyan^/yellow^/black}
	== {red^/green^/blue^/magenta^/cyan^/yellow}
	red>> call/input/console "grep y" data
	cyan
	yellow
	== 0
	red>>
```
Block examples :
```red
	red>> call/input/console "cat" [ "This" "is" "a" "Red" "world^/"  ]
	This is a Red world
	== 0
	red>>

	red>> a: "This is"
	== "This is"
	red>> b: "Red world"
	== "Red world"
	red>> call/input/console "cat" reduce [ a "a" b lf ]
	This is a Red world
	== 0
	red>>
```

#### **/output**

Process output is redirected to a string or a block.

If parameter is a string. CALL insert the redirected output before the beginning of this string :
```red
	red>> out: "" call/output {echo Welcome Red Language} out
	== 0
	red>> probe out
	"Welcome Red Language^/"
	== "Welcome Red Language^/"
	red>> call/output {echo Hello Red world} out
	== 0
	red>> probe out
	"Hello Red world^/Welcome Red Language^/"
	== "Hello Red world^/Welcome Red Language^/"
	red>>
```

If parameter is a block. CALL insert a new item containing redirected output as first item of this block.
```red
	red>> out: [] call/output {echo Welcome Red Language} out
	== 0
	red>> call/output {echo Hello Red world} out
	== 0
	red>> probe out
	["Hello Red world^/" "Welcome Red Language^/"]
	== ["Hello Red world^/" "Welcome Red Language^/"]
	red>>
```

Windows example :
```red
	red>> data: "" call/output {findstr "Nenad" *.r} data
	== 0
	red>> print data
	lexer.r:        Author:  "Nenad Rakocevic"
	lexer.r:        Rights:  "Copyright (C) 2011-2012 Nenad Rakocevic. All rights reserved."
	compiler.r:     Author:  "Nenad Rakocevic"
	compiler.r:     Rights:  "Copyright (C) 2011-2012 Nenad Rakocevic. All rights reserved."
	red.r:  Author:  "Nenad Rakocevic, Andreas Bolka"
	red.r:  Rights:  "Copyright (C) 2011-2012 Nenad Rakocevic, Andreas Bolka. All rights reserved."

	red>>
```

When output is redirected, the /console refinement as no effect.
```red
	red>> out: "" call/output/console {echo Welcome Red Language} out
	== 0
	red>>
```

#### **/error**

Same behavior as /output refinement. Parameter can be a string or a block.
```red
	red>> err: "" call/error "cp" err
	== 1
	red>> print err		;-- Language dependent message
	cp: missing file arguments Try `cp --help' for more information.
	red>>
```
#### **/shell**

Unix and MacOSX only.

RED identify the user shell and uses it to launch command with a "-c" option.

This refinement allows the use of shell's redirections symbols : "<" stdin, ">" stdout and "|" pipe.
```red
	red>> data: {white^/red^/green^/blue^/magenta^/cyan^/yellow^/black}
	== {white^/red^/green^/blue^/magenta^/cyan^/yellow^/black}
	red>> call/console/shell/input "grep a" data
	magenta
	cyan
	black
	== 0
	red>> call/console/shell/input "grep a | sort" data
	black
	cyan
	magenta
	== 0
	red>>
```

### Combining refinements

Redirections can be combined together.
```red
	red>> data: {white^/red^/green^/blue^/magenta^/cyan^/yellow^/black}
	== {white^/red^/green^/blue^/magenta^/cyan^/yellow^/black}
	red>> out: "" call/input/output "sort" data out
	== 0
	red>> probe out
	{black^/blue^/cyan^/green^/magenta^/red^/white^/yellow^/}
	== {black^/blue^/cyan^/green^/magenta^/red^/white^/yellow^/}
	red>>
```

## Return code

### Posix

With a /wait ot implicit wait refinement.
```red
	red>> call/wait "ls"
	== 0
	red>> call/wait "ts"	;-- ts is not a unix command
	== 255
```

## Unix parameters expansion

Unix and MacOSX version of CALL use POSIX [wordexp](http://pubs.opengroup.org/onlinepubs/9699919799/functions/wordexp.html) function to perform [wildcards](http://www.tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm) expansion and environment variables substitution.

```red
	red>> call/wait/console "ls *.r"
	compiler.r  lexer.r  red.r  run-all.r  version.r
	== 0
	red>> call/wait/console "ls [rc]*.r"
	compiler.r  red.r  run-all.r
	== 0
	red>> call/wait/console "echo $PATH"
	/home/local/bin:/usr/local/bin:/usr/bin
	== 0
	red>> call/wait/console "echo $SHELL"
	/bin/bash
	== 0
	red>>
```

Shell redirection is not performed by wordexp. The **/shell** refinement is required if you need pipes or redirection to file :
```red
	red>> call/wait/console "ls > ls.txt"
	Error Red/System call, wordexp parsing command : ls > ls.txt
	Use of the unquoted characters- <newline>, '|', '&', ';', '<', '>', '(', ')', '{', '}'
	== 255
	red>> call/wait/console/shell "ls > ls.txt"
	== 0
	red>>
```

## Windows issues

Windows' call implementation adds `cmd /u /c ` before the command you ask, **/u** ask for unicode chars,
**/c** execute command line and exit.

With the **/console** refinement, output is printed to the console with no encoding problem.

When grabbing output with **/output** or **/error** refinements there are a few encoding issues :

Found on http://social.msdn.microsoft.com :
*The Command Prompt (cmd.exe /U /k) - when used as the child process - which writes out Unicode, but does not accept Unicode.
Plus, there is the possibility that the child process can skip back and forth between Unicode and Ansi (cmd.exe /U /k ping google.com).
In that case ping.exe writes out Ansi.  Then when ping.exe exits and releases control to cmd.exe, the prompt returns in Unicode.*

Some commands will return ansi chars. **Call** detect this case and chars greater than #"^(7F)" are translated to space char.
The **dir** command returns wide-char not translated, the **tree** or **ping** command returns ansi chars translated.

## 32/64 bit issues
Since red is working in 32 bit environment, some calls need to be done differently.  Particularly files in `\Windows\System32` are not what you expect.  With file explorer you might find `c:\Windows\System32\OpenSSH\ssh.exe`. But that is a 64bit program and not visible for `red`, in fact the whole OpenSSH directory is missing.  The reason is that with a 32 bit progam you are not looking into `c:\Windows\System32` but redirected to `c:Windows\SysWoW64` and in that directory `OpenSSH` does not exist. (Naming conventions are really  confusing. One would expect System32 to be special for 32 bit programs!).

You can see what red sees by running the 32 bit cmd from `C:\Windows\SysWoW64\`.

Sample calls:
```
call/output "c:\windows\sysnative\cmd.exe /c dir c:\Windows\System32" o2: ""   print o2

<Add more as you see fit>
```

However, there is a solution to run the 64bit programs under `c:\Windows\System32` because the 64 bit version is mapped to `c:\Windows\sysnative\`. One reason that directory is hard to find is that it is not visible, you have to spell out the directory `sysnative`, tab completion does not help you.

So instead of calling `c:\Windows\System32\OpenSSH\ssh.exe` call `c:\Windows\sysnative\OpenSSH\ssh.exe`  and it should work.

# Call

Use `help call` to see all the options. For more detailed notes, here are some old Rebol docs. Red aims to be highly compatible, but also tries to improve things.

http://www.rebol.com/docs/shell.html
http://www.rebol.com/article/0004.html
https://stackoverflow.com/questions/41889113/rebol-3-reading-stdin-efficiently-line-by-line-to-make-awk-like-tool

Note that any references to system/port, e.g., in the R3 SO notes, won't work, as we don't have ports and full I/O yet. Soon though.

# Related Chats

https://rebol.tech/gitter.im/red/red/2021/#msg61b0ed16a9c8eb44c41d6810