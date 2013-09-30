Auteurs: Pierre (pierrechtux), Jocko

Alors voilà: vous avez entendu parler de Rebol depuis longtemps, comme un langage Rebolutionnaire. Peut-être même pratiquez-vous déjà Rebol avec joie?
Vous avez aussi entendu parler du langage Red, qui reprendrait la syntaxe de Rebol, conçu d'emblée de manière ouverte (libre, open source), qui progresse aujourd'hui (ces lignes s'écrivent au printemps 2013 de l'hémisphère Nord de la Terre) à grande vitesse.
Intrigué, émoustillé, vous voulez vous engouffrer, tester Red.

Mais voilà, comment faire?

/*English approximate translation:
So you have heard of Rebol from a long time, your neighbour keeps on telling you it's a Rebolutionary language.  Maybe you are already a happy Reboler?
You've also heard from Red, a language with the same syntax as Rebol, designed right from the start in an open manner (free-libre, open-souce), which is progressing at a lightning pace now (this is written at the end of summer 2013 from Earth's Northern Hemisphere).
So you're all excited, you want to bite the bullet and test Red.

But, how to do it?  Where to start?
*/


# Pour les Windowsiens
/*
# For the poor windows users
*/
Supposons que vous êtes sous Windows, et que vous avez déjà installé Rebol sur votre machine :

## Dans un premier temps, étudions red/system :
* chargez le zip de Red-Red/System et installez le dans le répertoire `Red`.
* créez tout de suite un répertoire `mes-progs`, au même niveau que `Red`, pour éviter la tentation de simplement compiler les exemples...
* créez dans ce répertoire votre premier programme:

<pre><code>Red/System [
        Title:  "mon premier programme"
        Author: "moi"
        File:   %hello-1.reds
]
print "hello, world !"
</code></pre>

* compilez:
<pre><code>do/args %../Red/red-system/rsc.r  "%hello-1.reds"
</code></pre>

L'exécutable se trouve dans Red\red-system\builds.

Le système a compilé un exécutable Dos. Ouvrir une console, et taper "hello-1". Voilà, content ?


Créons maintenant un exécutable Windows:

Red/system permet d'utiliser des librairies dynamiques. Nous allons appeler une dll windows, pour les besoins de notre exemple, sans nous attarder sur la syntaxe.

Voici notre deuxième programme :

	Red/System [
		Title:   "hello"
		Author:  "moi"
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
		alert "hello, world !"
		rep: confirm "quitter ?"	
		rep = 1
	] 
	


compilez, dégustez

	do/args %../Red/red-system/rsc.r  "%hello-2.reds" 



Nous avons, de nouveau, créé un programme Dos. La console est gênante ici. Nous allons générer maintenant un exécutable windows :

	do/args %../Red/red-system/rsc.r "-t Windows %hello-2.reds" 


On peut le lancer directement en cliquant dessus ...
C'est mieux, comme ça ...




/*
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
print "hello, Red world ! Are you Reddy?"
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

*/



## Abordons maintenant Red:

Voici la version Red de "hello, world!"
Remarquez l'en-tête ("Red"), et la fonction de retour (quit)
sauvegardez ce code avec l'extension .red

	Red[
	        Title:  "hello"
	        Author: "jocko"
	        File:   %hello-1.red
	]
	print "hello, world !"

	; quit est indispensable dans les programmes compilés sous DOS
	quit

Si la configuration des répertoires est la même que précédemment, la compilation se fait
en ouvrant une console rebol dans le répertoire mes-progs, et en tapant :

	do/args %../Red/red.r "%hello-1.red"

L'exécutable se trouve dans le répertoire mes-progs. Le lancer avec une console DOS.

Reprenons maintenant notre second programme, et convertissons-le en programme Red :

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

	; noter la conversion de string en c-string : 
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
			if rep = 6 [rep: 1]  ; sinon rep = 7
			rep
	]

	rep: 0

	until [
		alert "hello, world !"
		rep: confirm "quitter ?"    
		rep = 1
	] 

Plusieurs remarques:
- l'importation de librairies reste du code red/system. Pour le signaler, on utilise la directive #system
- les fonctions alert et confirm sont maintenant déclarées comme des routines, car elles comportent des appels à des fonctions red/system
- il y a une conversion à faire entre la représentation des strings sous red/system (c-string!), et sous Red (string!). D'où ce code ésotérique as c-string! string/rs-head 

Enregistrez sous hello-2.red . Compilez par :

	do/args %../Red/red.r "-t windows %hello-2.red"

et vous avez un exécutable windows, que l'on peut lancer en double cliquant dessus.



/*
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

A few comments:
- importing libraries is still red/system code.  To mention this, the #system directive is mentioned.
- alert and confirm functions are now declared as routines, since they make calls to red/system functions.
- a conversion must be done between the strings'representation in red/system (c-string!), and in Red (string!).  That's the reason for the esoteric code as c-string! string/rs-head 

Save as hello-2.red . Compile :

	do/args %../Red/red.r "-t windows %hello-2.red"

and you get a windows executable, which you can launch with a double-click.

*/

## REPL, une console d'évaluation interactive:
/*
## REPL, une console d'évaluation interactive:
*/

REPL signifie read-eval-print loop.

Parmi les exemples fournis dans le répertoire red/tests, il y a une console REPL, qui permet d'entrer et de tester de manière interactive des expressions Red. L'intérêt de cet outil justifie qu'on lui dédie (plus tard) une page de wiki.

En attendant, vous êtes maintenant armés pour compiler ... et essayer la console :

Nous supposons que vous êtes toujours dans votre répertoire mes-progs :

	do/args %../Red/red.r "%../Red/red/tests/console.red"

Vous trouverez l'exe dans ce même répertoire ...Lancez-le, une console s'ouvre. Entrez des expressions simples, avec la syntaxe rebol ...

Un dernier mot : soyez sympa, compilez aussi (sous windows !) ce programme pour vos amis, linuxiens invétérés :

	do/args %../Red/red.r "-t linux %../Red/red/tests/console.red"

Et aussi, pour les heureux possesseurs de Mac :

	do/args %../Red/red.r "-t darwin %../Red/red/tests/console.red"


# Pour les Linuxiens
Dans le coin supérieur droit de la page http://www.red-lang.org/ , un bandeau rouge oblique invite à forker.

Suivons ce conseil avisé, qui consiste à récupérer le code source de Red. Soit on suit le lien, soit, en ligne de commande:

      # pierre@autan: ~$        < 2013_04_30__18_37_46 >
    git clone https://github.com/dockimbel/Red.git
    Cloning into Red...
    remote: Counting objects: 13260, done.
    remote: Compressing objects: 100% (6701/6701), done.
    remote: Total 13260 (delta 6654), reused 13106 (delta 6510)
    Receiving objects: 100% (13260/13260), 6.03 MiB | 215 KiB/s, done.
    Resolving deltas: 100% (6654/6654), done.



Un répertoire Red a été créé dans le répertoire courant.

Il faut maintenant un exécutable Rebol pour permettre la compilation de Red; je prends ici un Rebol/view 2 pour Linux x86 Libc6, je désarchive le tarball, et je copie l'exécutable là où il convient pour Red:

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
    

Dorénavant, nous avons la même architecture, pour Linuxiens et Windowsiens.

# Pour les Macqueux