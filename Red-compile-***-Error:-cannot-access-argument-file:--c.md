### *** Error: cannot access argument file:  -c
the error messages a few people over the years have seen and posted about.
it appears to affect very few people and is related to not being able to process commandline options.
noticeable because you won't be able to -c compile or -e encap your programs.
affects both the red gui and regular console

please list details of the OS, commandline and how the red executable was obtained
maybe there will emerge a common denominator
* for example: does --help work, see `red/usage.txt` for other commandline options.

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
 set to minimim but not off.
### red version how and where obtained. 
 built from source every month or less using rebol2
 I build them in the red-master source dir which is not on the path so I give the full path to run them
from another directory using something like total commander or cmd.exe or a short cut
#### commandline and invoked from: 
tried anything and everything. I can build the source/test/hello.red with rebol
but this doesn't seem to work with my programs even if I move them into the source dir  
--ne1  
****

#### latest reference 
[17th of Feb 2018](https://gitter.im/red/red?at=5a887e746fba1a703a64e8ec) on win10 and win7 or win8 

#### archlinux packages, linked from the download page, comments mentions arg file error:
+ also: are the details of building red with rebol needing SDK accurate? 
+ https://aur.archlinux.org/packages/red/

#### almost any wrong commandline option might trigger the error message
+ https://stackoverflow.com/search?q=Error%3A+cannot+access+argument+file

#### at least two tickets unrelated to compiling that mention the same error:
* Linux: cannot access argument file · Issue #2426 · red/red
+ https://github.com/red/red/issues/2426

* *** Error: cannot access argument file - When I start in Wine under Ubuntu 15.10 · Issue #1602 · red/red
+ https://github.com/red/red/issues/1602

****
**Note:** Windows does not recognize the `%20` separator in directories. This might be a contributing factor.

