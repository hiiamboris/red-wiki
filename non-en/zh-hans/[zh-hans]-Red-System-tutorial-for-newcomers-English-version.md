Authors: Pierre (pierrechtux), Jocko

So you have been hearing of Rebol for a long time, your neighbour keeps on telling you it's a Rebolutionary language.  Maybe you are already a happy Reboler?
You've also heard from Red, a language with the same syntax as Rebol, designed right from the start in an open manner (free-libre, open-source), which is progressing at a lightning pace now (this is written at the end of summer 2013 from Earth's Northern Hemisphere).
So you're all excited, you want to bite the bullet and test Red.

But, how to do it?  Where to start?


# For the poor windows users

Let's suppose that you paid a windows licence, and that you already have Rebol installed on your machine:

## First of all, let's see red/system :
* download the .zip containing Red-Red/System and put it in the `Red` directory.
* create now a directory named 'my-progs', at the same level as `Red`, just not to be tempted of /only compiling examples...
* create your first Red program in this directory:

<pre><code>Red/System [
        Title:  "my first program"
        Author: "me"
        File:   %hello-1.reds
]
print "Hello, Red world! Are you Ready?"
</code></pre>


* compile it:
<pre><code>do/args %../Red/red-system/rsc.r  "%hello-1.reds"
</code></pre>

The executable can be found in Red\red-system\builds.

The system compiled a DOS executable.  Open a console, and type "hello-1".  Happy?



Now let's do a Windows executable:

Red/system allows to use dynamic libraries.  As an example, We will call a windows dll, without worrying too much about syntax.

Here is our second program :


	Red/System [
		Title:   "hello"
		Author:  "me"
		File: 	 %hello-2.reds
	]
	
	; --- lib imports -----
	
	#import [
		"user32.dll" stdcall [
			MessageBox: "MessageBoxA" [
				handle		[integer!] 
				text		[c-string!] 
				title		[c-string!]
				type 		[integer!]
			return:	[integer!]
			]
	  	]
	]
	
	alert: func [txt [c-string!] return: [integer!]][
		MessageBox 0 txt "alert" 48 
		]
		
	confirm: func [txt [c-string!] return: [integer!] /local rep [integer!]][
		rep: MessageBox 0 txt "confirm" 4 
		if rep = 6 [rep: 1]  ; sinon rep = 7
		rep
	]
	
	rep: 0
	
	until [
		alert "hello, Red world !"
		rep: confirm "quit ?"	
		rep = 1
	] 
	


compile, run

	do/args %../Red/red-system/rsc.r  "%hello-2.reds" 



Again, we made a DOS program; console is a bit in the way.  Now, let's try to do proper windows executable :

	do/args %../Red/red-system/rsc.r "-t Windows %hello-2.reds" 


Now you can launch it with a simple click...
That's better.


## Let's talk about Red:

Here is the Red version from "hello, world!"
Notice the header ("Red"), and the return function (quit).
Save this code with a .red extension.

	Red[
	        Title:  "hello"
	        Author: "jocko"
	        File:   %hello-1.red
	]
	print "hello, world !"

	; quit is mandatory for programs compiled with DOS
	quit

Provided that the directory hierarchy is the same as previously, compile is done by opening a rebol console in the my-progs directory, and by entering:

	do/args %../Red/red.r "%hello-1.red"

The resulting executable is located in the my-progs directory.  Launch it from a DOS console.

Now let's re-edit our second program, and let's convert it into a Red program:

	Red[
			Title:  "hello"
			Author: "jocko"
			File:   %hello-2.red
	]
	; --- lib imports -----
	#system[
		#import [
			"user32.dll" stdcall [
				MessageBox: "MessageBoxA" [
					handle      [integer!] 
					text        [c-string!] 
					title       [c-string!]
					type        [integer!]
				return: [integer!]
				]
			]
		]
	]

	; note the conversion from string to c-string : 
	; as c-string! string/rs-head txt

	alert: routine [
				txt [string!] 
				return: [integer!]][
			MessageBox 0 as c-string! string/rs-head txt "alert" 48 
	]

	confirm: routine [
				txt [string!] 
				return: [integer!] 
				/local rep [integer!]][
			rep: MessageBox 0 as c-string! string/rs-head txt "confirm" 4 
			if rep = 6 [rep: 1]  ; otherwise rep = 7
			rep
	]

	rep: 0

	until [
		alert "hello, red world !"
		rep: confirm "quit ?"    
		rep = 1
	] 

# Using #include for a Red/System file from Red

This is how you can include a Red/System file in a Red program:

Red Code:

```red
Red []

print "From Red"

#system [
    #include %../Red-System/test.reds
    f
]

print "From Red"

r: routine [][f]
r

print "From Red"
```

Red/System code:

```red
Red/System []

f: func [] [
    print ["From Red System" lf]
]
```

A few comments:
- importing libraries is still red/system code.  To mention this, the #system directive is mentioned.
- alert and confirm functions are now declared as routines, since they make calls to red/system functions.
- a conversion must be done between the strings'representation in red/system (c-string!), and in Red (string!).  That's the reason for the esoteric code as c-string! string/rs-head 

Save as hello-2.red . Compile :

	do/args %../Red/red.r "-t windows %hello-2.red"

and you get a windows executable, which you can launch with a double-click.


## REPL, taking control with the interactive console:

REPL stands for read-eval-print-loop.  An acronymic name for an interactive interpreter.  Like the Rebol console, yes.

Among examples provided in the red/tests directory, there is a REPL console, which allows to enter and interactively test Red expressions.  This tool is so valuable that it deserves a whole wiki page (later on).

Now you are all setup to compile... and try the console:

Let's suppose that you still are in your my-progs directory :

	do/args %../Red/red.r "%../Red/red/tests/console.red"

You'll find the executable in this same directory: launch it, a console opens.  Enter some simple expressions, with the rebol syntax ...

A last word: be humanists, also compile (under windows !) this program for your Linux-users friends :

	do/args %../Red/red.r "-t linux %../Red/red/tests/console.red"

And also, for the happy Mac users :

	do/args %../Red/red.r "-t darwin %../Red/red/tests/console.red"


# For GNU/Linux users
In the upper left corner of http://www.red-lang.org/ webpage, a Red oblique sign invites you to fork.

Just follow this wise advice, which makes you get Red source code.  Either you follow the link, or, on the command line :

      # pierre@autan: ~$        < 2013_04_30__18_37_46 >
    git clone https://github.com/dockimbel/Red.git
    Cloning into Red...
    remote: Counting objects: 13260, done.
    remote: Compressing objects: 100% (6701/6701), done.
    remote: Total 13260 (delta 6654), reused 13106 (delta 6510)
    Receiving objects: 100% (13260/13260), 6.03 MiB | 215 KiB/s, done.
    Resolving deltas: 100% (6654/6654), done.



A Red directory was just created in the current directory.

You now need a Rebol executable to compile Red; here, I use a Rebol/view 2 for Linux x86 Libc6, I unpack the tarball, and copy the executable on a suitable place for Red:

    # pierre@autan: ~$ < 2013_04_30__18_37_46 >
    wget http://www.rebol.com/downloads/v278/rebol-view-278-4-2.tar.gz
    --2013-04-30 21:17:15-- http://www.rebol.com/downloads/v278/rebol-view-278-4-2.tar.gz
    Résolution de www.rebol.com... 205.134.252.23
    Connexion vers www.rebol.com|205.134.252.23|:80...connecté.
    requête HTTP transmise, en attente de la réponse...200 OK
    Longueur: 653165 (638K) [application/x-gzip]
    Sauvegarde en : «rebol-view-278-4-2.tar.gz»
    
    100%[======================================>] 653,165 168K/s ds 4.2s
    
    2013-04-30 21:17:20 (151 KB/s) - «rebol-view-278-4-2.tar.gz» sauvegardé [653165/653165]
    
      # pierre@autan: ~$        < 2013_04_30__18_37_46 >
    tar zxf rebol-view-278-4-2.tar.gz
       
      # pierre@autan: ~$        < 2013_04_30__18_37_46 >
    cp releases/rebol-view/rebol Red/red-system/
    

And now, we all share the same directory structure, Linux users and windows users.


# For Mac users

My wife won't let me use here Mac.  If I can get a hold on it, I'll try to write something.  In the mean time, check Arnold's very good videos, they feature a Red Mac: http://www.youtube.com/playlist?list=PLr1rbtkaZDGDKtExuz8Q0nFDtDtUyyOHz&feature=c4-feed-u

# See Also

http://red.qyz.cz/red-system-from-red.html




