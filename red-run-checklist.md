## red won't run?

 *  are you running latest version?
	* stable version is old
	* many problems have been fixed since then
	* is it copied to a path that can be found?


* you've downloaded latest from red-lang
	* or
* you've cloned or downloaded source from github.com/red/red 
	*  and rebol and built the gui console


 *  which OS
	* windows, has your Anti-Virus quarantined?
	
	* linux,  using wine or some virtual machine
		*	installed 32bit libs?
		
	* mac, newer versions require 64 bit so need a virtual solution
		* red is 32 bit at this time,
		 	see prepared docker links on red wiki
		 	also, unofficial
		```
		https://hub.docker.com/r/kskarthik/redlang
		https://gist.github.com/refaktor/d05d550d20fb882c74c70104fd929bfb
		https://github.com/otobrglez/compression-puzzle/pull/39/files

		```
		

## red still won't run
	*	does red --cli work?
	*	have previous versions worked?
	* run red from a terminal to see any error messages
	* check event log for errors

 * windows 7, 10 64bit or 32 bit,  run dxdiag
	*	using old computer with limited gpu?
	*  dxdiag version should be 11 or greater
	*     update drivers or video card 
	*	sorry, you might be stuck running older version pre 2021
	* see [red/issues/GUI console doesn't start #5073](https://github.com/red/red/issues/5073)


[gitter red/welcome](https://gitter.im/red/red/welcome) or help chat for more assistance
```
	the game of 20 questions is really no fun for anyone.
	* what is your OS?
	* which red did you try to run?
	* have you tried:  red --cli
	* has your AV quarantined the executable?
	* are you able to try another computer?
	* for Mac and linux users, red only works in 32 bit mode.
		* Mac users can run a docker or VM
		* linux users can usually install 32 bit libs
		* 	see the download page

	* does help work in the console?
```
<img SEO="red won't run, problem running red, error red.exe, why red disappear, red checklist">