# How to retrieve default Windows locales values

We have to write (at june 2019, Red 0.6.4) a simple code in Red/System (to compile with -r flag)
to access Windows API to get the locales values we need. 
The followings are Microsoft API docs links:
- https://docs.microsoft.com/en-us/windows/desktop/api/winnls/nf-winnls-getuserdefaultlcid
- https://docs.microsoft.com/it-it/windows/desktop/api/winnls/nf-winnls-getuserdefaultlocalename
- https://docs.microsoft.com/it-it/windows/desktop/api/winnls/nf-winnls-getlocaleinfoex
- https://www.pinvoke.net/default.aspx/kernel32.GetLocaleInfoEx

For the u16len? function, used in the following example code, this is the credit link 
https://github.com/NjinN/Recode/blob/master/Red/flappyBird/mci-cmd.red

**The followings are three examples codes:**
***
> 1) Get user default local name as Red string! with API GetUserDefaultLocaleName

> Example of retrived value: returned Red string : it-IT

```red
Red[
	Note:	 {Red 0.6.4 compiled with -r flag - tested on Win10 x64}
]
#system [
	g-loc: context [
		#define LOCALE_NAME_MAX_LENGTH  85
		#import [
			"kernel32.dll" stdcall [
				get-user-dname: "GetUserDefaultLocaleName"[
				lpLocaleName  [c-string!]
				cchLocaleName [integer!]
				return:       [integer!]
				]
			]
 			"msvcrt.dll" cdecl [
				malloc: "malloc" [
					size    [integer!]
					return: [c-string!]
				]
				release: "free" [
					block   [c-string!]
				]
			]		
		]
		u16len?: func [s [c-string!] return: [integer!] /local len [integer!] index-next [integer!]][
			len: -1
			until [
				len: len + 2
				index-next: len + 1
				all [s/len = #"^@"	s/index-next = #"^@"]
			]
			return len / 2 
		]		
		get-locale: function [/local buffer rs [red-string!]] [	
			buffer: malloc  256
			;Win API-> GetUserDefaultLocaleName	
			either (get-user-dname buffer LOCALE_NAME_MAX_LENGTH) = 0 [
				rs: string/load "error" 5 UTF-8
				][
				rs: string/load buffer u16len? buffer UTF-16LE
			]
			release buffer
			SET_RETURN(rs)
		] 
	]
]	
l-n: routine [] [g-loc/get-locale]
locale-name: l-n 
print "GetUserDefaultLocaleName"
print ["returned Red" type? locale-name ": " locale-name]

```
***
***
> 2) Get user default local language as Red block!

> Example of retrived values: returned Red block : [1040 "it-IT" "italiano" "Italiano (Italia)" "Italian (Italy)"]

```red
Red[
	Note: 	 {Red 0.6.4 compiled with -r flag - tested on Win10 x64}
]
#system [
	g-loc: context [
		#define LOCALE_NAME_MAX_LENGTH  85
		#define LOCALE_NAME_USER_DEFAULT      null
		#define LOCALE_RETURN_NUMBER          20000000h   ;-- return number instead of string
		#define LOCALE_SENGLISHDISPLAYNAME    00000072h   ;-- Display name (language + country usually) in English, eg 
		#define LOCALE_SLOCALIZEDDISPLAYNAME  00000002h   ;-- localized name of locale, eg "German (Germany)" in UI language
		#define LOCALE_SNATIVELANGUAGENAME    00000004h   ;-- native name of language, eg "Deutsch"
		;
		#import [
			"kernel32.dll" stdcall [
				get-user: "GetUserDefaultLCID" [
					return: [integer!]
				]
				get-user-dname: "GetUserDefaultLocaleName" [
					lpLocaleName  [c-string!]
					cchLocaleName [integer!]
					return:       [integer!]
				] 
				get-locale-info: "GetLocaleInfoEx" [
					lpLocaleName [c-string!] ; LPCWSTR Long Pointer to Constant Wide String
					LCType       [integer!]
					lpLCData     [c-string!] ; LPWSTR
					cchData      [integer!]  
					return:      [integer!]
				]
			]
 			"msvcrt.dll" cdecl [
				malloc: "malloc" [
					size    [integer!]
					return: [c-string!]
				]
				release: "free" [
					block   [c-string!]
				]
			]		
		]
		u16len?: func [s [c-string!] return: [integer!] /local len [integer!] index-next [integer!]] [
			len: -1
			until [
				len: len + 2
				index-next: len + 1
				all [s/len = #"^@"	s/index-next = #"^@"]
			]
			return len / 2 
		]	
	  	set-value: function [result [integer!] buffer [c-string!] vals [red-block!]] [
			either result = 0 [string/load-in "error" 5 vals UTF-8] [
				string/load-in buffer u16len? buffer vals UTF-16LE	
			]
		]
		get-locale: function [/local buffer vals [red-block!] result [integer!] rs [red-string!] ] [	
			buffer: malloc  256
			vals: block/make-in null 5
			;
			;Win API-> GetUserDefaultLCID 
			result: get-user ;-- return 1040 x it
			integer/make-in vals result
			;
			;Win API-> GetUserDefaultLocaleName
			result: get-user-dname buffer LOCALE_NAME_MAX_LENGTH
			set-value result buffer vals

			;Win API-> GetLocaleInfoEx LOCALE_SNATIVELANGUAGENAME
			result: get-locale-info LOCALE_NAME_USER_DEFAULT LOCALE_SNATIVELANGUAGENAME buffer  256
			set-value result buffer vals

			;Win API-> GetLocaleInfoEx LOCALE_SLOCALIZEDDISPLAYNAME
			result: get-locale-info LOCALE_NAME_USER_DEFAULT LOCALE_SLOCALIZEDDISPLAYNAME buffer 256
			set-value result buffer vals

			;Win API-> GetLocaleInfoEx LOCALE_SENGLISHDISPLAYNAME
			result: get-locale-info LOCALE_NAME_USER_DEFAULT LOCALE_SENGLISHDISPLAYNAME buffer 256
			set-value result buffer vals

			release buffer		
			SET_RETURN(vals)
		] 
	]
]	
l-n: routine [] [g-loc/get-locale]
locale-name: l-n 
print ["returned Red" type? locale-name ": "]
probe locale-name
print ""
print ["GetUserDefaultLCID: " locale-name/1]
print ["GetUserDefaultLocaleName: " locale-name/2]
print ["LOCALE_SNATIVELANGUAGENAME: " locale-name/3]
print ["LOCALE_SLOCALIZEDDISPLAYNAME: " locale-name/4]
print ["LOCALE_SENGLISHDISPLAYNAME: " locale-name/5]
print "---end---"

```
***
***
> 3) Get user default local days and months as Red block!

> Example of retrived values: 
returned Red block :
["lunedì" "martedì" "mercoledì" "giovedì" "venerdì" "sabato" "domenica" "lun" "mar" "mer" "gio" "ven" "sab" "dom"]

> returned Red block :
["gennaio" "febbraio" "marzo" "aprile" "maggio" "giugno" "luglio" "agosto" "settembre" "ottobre" "novembre" "dicembre"]

```red
Red[
	Note:	 {Red 0.6.4 compiled with -r flag - tested on Win10 x64}	
]
#system [
	g-loc: context [
		#define LOCALE_NAME_USER_DEFAULT      null
		;
		#define	LOCALE_SDAYNAME1  0000002Ah   ;-- long name for Monday
		#define	LOCALE_SDAYNAME2  0000002Bh
		#define	LOCALE_SDAYNAME3  0000002Ch
		#define	LOCALE_SDAYNAME4  0000002Dh
		#define	LOCALE_SDAYNAME5  0000002Eh
		#define	LOCALE_SDAYNAME6  0000002Fh
		#define	LOCALE_SDAYNAME7  00000030h
		;
		#define	LOCALE_SABBREVDAYNAME1  00000031h   ;-- abbreviated name for Monday
		#define	LOCALE_SABBREVDAYNAME2  00000032h
		#define	LOCALE_SABBREVDAYNAME3  00000033h
		#define	LOCALE_SABBREVDAYNAME4  00000034h
		#define	LOCALE_SABBREVDAYNAME5  00000035h
		#define	LOCALE_SABBREVDAYNAME6  00000036h
		#define	LOCALE_SABBREVDAYNAME7  00000037h
		;
		#define LOCALE_SMONTHNAME1  00000038h   ;-- long name for January
		#define LOCALE_SMONTHNAME2  00000039h
		#define LOCALE_SMONTHNAME3  0000003Ah
		#define LOCALE_SMONTHNAME4  0000003Bh
		#define LOCALE_SMONTHNAME5  0000003Ch
		#define LOCALE_SMONTHNAME6  0000003Dh
		#define LOCALE_SMONTHNAME7  0000003Eh
		#define LOCALE_SMONTHNAME8  0000003Fh
		#define LOCALE_SMONTHNAME9  00000040h
		#define LOCALE_SMONTHNAME10 00000041h
		#define LOCALE_SMONTHNAME11 00000042h
		#define LOCALE_SMONTHNAME12 00000043h
		;
		#define LOCALE_SABBREVMONTHNAME1  00000044h  ;-- abbreviated name for January
		#define LOCALE_SABBREVMONTHNAME2  00000045h
		#define LOCALE_SABBREVMONTHNAME3  00000046h
		#define LOCALE_SABBREVMONTHNAME4  00000047h
		#define LOCALE_SABBREVMONTHNAME5  00000048h
		#define LOCALE_SABBREVMONTHNAME6  00000049h
		#define LOCALE_SABBREVMONTHNAME7  0000004Ah
		#define LOCALE_SABBREVMONTHNAME8  0000004Bh
		#define LOCALE_SABBREVMONTHNAME9  0000004Ch
		#define LOCALE_SABBREVMONTHNAME10 0000004Dh
		#define LOCALE_SABBREVMONTHNAME11 0000004Eh
		#define LOCALE_SABBREVMONTHNAME12 0000004Fh
		;
		#import [         
			"kernel32.dll" stdcall [
				get-locale-info: "GetLocaleInfoEx" [
					lpLocaleName [c-string!] ; LPCWSTR Long Pointer to Constant Wide String
					LCType       [integer!]
					lpLCData     [c-string!] ; LPWSTR
					cchData      [integer!]  
					return:      [integer!]
				]
			]
 			"msvcrt.dll" cdecl [
				malloc: "malloc" [
					size    [integer!]
					return: [c-string!]
				]
				release: "free" [
					block   [c-string!]
				]
			]		
		]
		u16len?: func [s [c-string!] return: [integer!] /local len [integer!] index-next [integer!]] [
			len: -1
			until [
				len: len + 2
				index-next: len + 1
				all [s/len = #"^@"	s/index-next = #"^@"]
			]
			return len / 2 
		]	
		;
		get-locale: function [type [integer!] /local buffer vals [red-block!] result [integer!] 
							  i [integer!] e days months] [	
			buffer: malloc  256
			vals: block/make-in null 5
			;
			days: [
				LOCALE_SDAYNAME1 LOCALE_SDAYNAME2 LOCALE_SDAYNAME3 LOCALE_SDAYNAME4
				LOCALE_SDAYNAME5 LOCALE_SDAYNAME6 LOCALE_SDAYNAME7
				LOCALE_SABBREVDAYNAME1 LOCALE_SABBREVDAYNAME2 LOCALE_SABBREVDAYNAME3
				LOCALE_SABBREVDAYNAME4 LOCALE_SABBREVDAYNAME5 LOCALE_SABBREVDAYNAME6
				LOCALE_SABBREVDAYNAME7
			]
			months: [
				LOCALE_SMONTHNAME1 LOCALE_SMONTHNAME2 LOCALE_SMONTHNAME3 LOCALE_SMONTHNAME4
				LOCALE_SMONTHNAME5 LOCALE_SMONTHNAME6 LOCALE_SMONTHNAME7 LOCALE_SMONTHNAME8
				LOCALE_SMONTHNAME9 LOCALE_SMONTHNAME10 LOCALE_SMONTHNAME11 LOCALE_SMONTHNAME12
			]				   
			;Win API-> GetLocaleInfoEx
			i: 1
			either type = 0 [e: days/0][e: months/0]
			until [
				either type = 0 [
					result: get-locale-info LOCALE_NAME_USER_DEFAULT days/i buffer  256
					][
					result: get-locale-info LOCALE_NAME_USER_DEFAULT months/i buffer  256
				]
				;
				either result = 0 [
					string/load-in "error" 5 vals UTF-8
					][
					string/load-in buffer u16len? buffer vals UTF-16LE
				]
				;
   				i: i + 1
				i > e   
			]
			;
			release buffer
		    SET_RETURN(vals)
		] 
	]
]
l-n: routine [type [integer!]] [g-loc/get-locale type]
locale-name: l-n 0
print ["returned Red" type? locale-name ": "]
probe locale-name
print ""
locale-name: l-n 1
print ["returned Red" type? locale-name ": "]
probe locale-name
print "---end---"

```
***
***
With these set of Microsoft API is possible to retrive a lot of locale values.
***



