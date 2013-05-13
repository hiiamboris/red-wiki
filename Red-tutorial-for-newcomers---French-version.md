Alors voilà: vous avez entendu parler de Rebol depuis longtemps, comme un langage Rebolutionnaire. Vous avez aussi entendu parler du langage Red, qui reprendrait la syntaxe de Rebol, conçu d'emblée de manière ouverte (libre, open source), qui progresse aujourd'hui (ces lignes s'écrivent au printemps 2013) à grande vitesse.
Intrigué, émoustillé, vous voulez vous engouffrer, tester Red.

Mais voilà, comment faire?

# Pour les Windowsiens
Supposons que vous être sous Windows, et que vous avez déjà installé Rebol sur votre machine :

Dans un premier temps, étudions red/system :
* chargez Red-Red/System et installez le dans un répertoire de votre choix.
* créez tout de suite un répertoire "mes-progs", au même niveau que red-master, pour éviter la tentation de simplement compiler les exemples ...
* créez dans ce répertoire votre premier programme:
    Red/System [
    	Title:   "mon premier programme"
    	Author:  "moi"
    	File: 	 %hello-1.reds
    ]
    print "hello, world !" 

* compilez:
    do/args %../Red-master/red-system/rsc.r  "%hello-1.reds"

L'exécutable se trouve dans Red-master\red-system\builds.

Le système a compilé un exécutable Dos. Ouvrir une console, et taper "hello-1". Voilà, content ?


Toujours sous Windows:

Red/system permet d'utiliser des librairies dynamiques.
Votre deuxième programme :

Red/System [
	Title:   "hello"
	Author:  "moi"
	File: 	 %hello-2.reds
]

; --- lib imports -----

#import [
	"user32.dll" stdcall [
		MessageBox: "MessageBoxA" [
			handle		[integer!] ;__in_opt  HWND hWnd,
			text		[c-string!] ;__in_opt  LPCTSTR lpText,
			title		[c-string!] ; __in_opt  LPCTSTR lpCaption,
			type 		[integer!]	; __in      UINT uType
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

  do/args %../Red-master/red-system/rsc.r  "%hello-2.reds" 



La console est gênante ici. Nous allons générer maintenant un exécutable windows :

  do/args %../Red-master/red-system/rsc.r "-t Windows %hello-2.reds" 



On peut le lancer directement en cliquant dessus ...
C'est mieux, comme ça ...




# Pour les Linuxiens
# Pour les Macqueux
