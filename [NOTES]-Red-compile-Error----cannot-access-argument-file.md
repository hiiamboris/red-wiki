### *** Error: cannot access argument file:  -c

an error messages a few people over the years have seen and posted about.
it appears to affect very few people 

one common way is related to not being able to process commandline options.
this happens if you build an executable from source. noticeable because you won't be able to -c compile or -e encap your programs. affects both the red gui and regular console because the official doanload executable includes encaped version of the rebol2 interpreter
>for explanation and workaround see: https://stackoverflow.com/questions/48178976/compile-red-and-red-system-compilers-from-source

>also, early 2018 Carl, the creator of Rebol which Red is based on, agreed to release the command version of Rebol 2 which should make it possible for anyone still without the SDK, which can no longer be purchased, to build a fully operational Red. thanks!
> see: http://www.red-lang.org/2018/03/red-rebol-carl.html


*see below for other reasons you see this error from the official version or a rebol2 built SDK version


please list details of the OS, commandline and how the red executable was obtained
maybe there will emerge a common denominator
* for example: does --help work, see `source/red/usage.txt` for other commandline options.

****
#### OS version:
#### VM?:
#### user level access:
#### red version how and where obtained:
#### commandline and invoked from:

****
#### OS version:
 win7 pro 64
#### VM?:
 no
#### user level access.
 an admin account but I still have to run as admin quite a few things
#### if windows UAC:
 set to minimum but not off.
### red version how and where obtained. 
 built from source every month or less using rebol2
 I build them in the red-master source dir which is not on the path so I give the full path to run them
from another directory using something like total commander or cmd.exe or a short cut
#### commandline and invoked from: 
tried anything and everything. I can build the source/test/hello.red with rebol
but this doesn't seem to work with my programs even if I move them into the source dir  
--ne1  
*resolved, was unaware building red from source while not self hosted (v1.0) will not be fully operational
****

#### latest reference 
[17th of Feb 2018](https://rebol.tech/gitter.im/red/red/2018/#msg5a887e746fba1a703a64e8ec) on win10 and win7 or win8 

#### archlinux packages, linked from the download page, comments mentions arg file error:
+ also: explains the details of building red with rebol needing SDK.
+ https://aur.archlinux.org/packages/red/

#### almost any wrong commandline option might trigger the error message
* for example: does --help work, see `source/red/usage.txt` for other commandline options.
+ https://stackoverflow.com/search?q=Error%3A+cannot+access+argument+file

#### at least two tickets unrelated to compiling that mention the same error:
* Linux: cannot access argument file · Issue #2426 · red/red
+ https://github.com/red/red/issues/2426

* *** Error: cannot access argument file - When I start in Wine under Ubuntu 15.10 · Issue #1602 · red/red
+ https://github.com/red/red/issues/1602

****
**Note:** Windows does not recognize the `%20` separator in directories. This might be a contributing factor.

