I make a `sequence diagram` from `do/args -r %red.r ...` to compiler a Red script, using [PlantUML](http://plantuml.com/sequence-diagram), in order to see how Red runtime starts.

Here is the PlantUML source, it must exists many mistakes. Wish you could correct my mistakes, thanks!

You can change the source below and copy to [PlantUML online server](http://www.plantuml.com/plantuml/uml) to regenerate a new diagram.

Now I know the Red runtime is stars from [runtime/red.reds](https://github.com/red/red/blob/master/runtime/red.reds#L139), and it's memory model is here [runtime/allocator.reds](https://github.com/red/red/blob/master/runtime/allocator.reds#L84).

Go Red!

## PlantUML source code link

[link](http://www.plantuml.com/plantuml/uml/tLXjSzku4Vui_egDCkxPcb2Kx7CSwowAw-GyxKokpTZfz8EYkK0aBE5C0Ie0MdR7FxwB5r8WHThvNCxOee1zTXVx3bqeJQGkrdcaxXZV44dMe9dEwQMG6PNNek3PHo4vbJ0-RQVvkcBf7QTAmN4xmKUH2mcIPdlR9BrRIYV2aFprFmM9FvACwikye96bWMkQGIhM6uRl6obI8uZm36J1DLjJO9nfHl9l_1k_98g2u1Oo6huTdXokjDweIP8icLuLYo7oIL0F_vTJf5T71db3wGGCCT9kjziX4iGvuSkXaClaU3GU9wDJWrAFIjDrxFMHiRYdyfxHxN2bqOJ1-RwWLLdADeJhBoJB65_2yIZOlnGFvVfu54KarNX-rBj8gOoYXNY0lZ_u6WyRvunJk9M2wvZoh0-lFxFboba9yDdif44VrHnASoXyIIV8WclweFj05DYj23a0IgYBiUVfxtChjogMUnnB2jXddDnOU-tum-jFHDxTa7mj-BuiPzsaBjARF4CVqrGtfAjAE3wT3D5wToKxzrvduzOwuNJDj2xn_akTWL_iLa1WK9SAsFU60dQj6quVAhHvRO3DKYUsq2nNQ845kkFwQsa3zwrfXPAc9K8QsOQElBgwnASb_1nbOcCDCwOhRAJknf8y5od1bB0SIeHTtUb62YruiLwWy_k_tY9pb81Lv3rIJdzTJ6yqtSGZW7xaqmhYgKi0cE3dGkJJAMGY8NAfuC1jzEFObmKuq5HfbQme2ZLQzAFewUddqwUdwFrx-8p-ge8Qj11vkYAC7uwF9hlbnCSLllZuWFVleyX9ShfKj1Eu69yPTUAKf2kKlik1AAGpC5L3ZKBKNW950JLYlK86Tzw2A_CaydJnxFK8dwkhULEkfBgG71h9r91MdXh57RUfMULLL5A-AB2PVQ5JlaYAmUGQnYvSqxp8g7AMRzYY37UhuBX5mMRkH05cZyFuP0gAQfZT5ZnLS4-acjl4VfPfJ1ysGcfb4jChQm8ZELm8eU4lW8s12QxmZHFDxgbvmvQTtWs7mwEQFY_xgf7qFnBGnnZGhZYPS3Q5fisIZi1OyRpyDa5hme9XhhQOe2o7y9EY-MtayOQJbR0Nrhx-HMwxTj9ATddAMdsiMBzed7fHhXsmXgtbh0TtTAZGldztPJCSPYI8ZVEjhmgr9A_9CTg1q2lZcXME32hYGkEaWyD7I1dr-eD1JS8US33eHprlepO7UKjxn_Jm518ZKBKJkqZsD2A0eKv8FEbrWnp53h71t7MpdR2QUUfRFCMan8Gq60nx60WNK2riALcIOZNdeGA69N31gMqaneuDupVSFGmS7RQnqBiV_NXgRAy518DFi-2xKl7kREHSdAmnoRxnYtUdnrDWjxYcYbnNFGIKJ4sKO0-zE3i_ioISOaUCosJ-OBTXjYXOdbMhTd7UsMbEn--cbIXJR2fngMGRRT7FTvsVtvwTM6sCDJyucyxgQO1CS_E93LM9Gg9Np6cICUo-6zYyKKjQ4woi6qHXWRN1CsIf0BOcI_fyvg2cvsDdpvWeBBRMfXtfOyQHmU1xGFozr810RPh58eqlcvmW6tCNOUY3QnlIWq4KKCw4P4lC1FW95-TWT4tC10M_30OiA0c-NI5uYnSLenJC3CsYqF3ll_rs_HimXHbCuRkqwMqZy0EI6vLHpzsO3pExPfuuU9wWV_Eunfa7XwakWiyCEoj1YYw2JpJDyV9YYrOdqHLsl-SfViUODpUv9Tw37D4yck-oyKUJiTiX2_HijDVTZ9_I856zpT_0akBjrWAjAGjD5aXIpznQzBypmy9UjDlznDTqr-AoAe5zkZM3lSRsv7pqTkf2rUiMevLoiJGr1JzO-WeyMvo4OURV_nIBE2EQ92Vp7s3lKdm26xopm-oW7DBXO3XCcfFx72XFoUP3PxklipIQ-Ct4r-54R7bPLTn7qEpj7Q3iRdZBVVGzgZR0sN8NET80KuvApyDgQOwuw7M9EyZehKr0z9KzQx2r9gaKUwicjcw5N2TRna-Ecxlr2qie1psZ7pTK9KhB8iN8fQgRfdpROv-sQkf7mkQocm_hYpWkrXkUiMVB_1GQqTHdLJciOtAgzyHd0h1yDM7sOw7o0hCozq8tLpJsIe7NEX_IvdSKmPrD_8uFSl-rkkUf2AlKrFx3gSqFJriXC_N611fsCRnNmfPX1dfPR-n9tjYx2IneBhRDslH2ImYADxffRWh_Fv39biAAtDEzkezbl-mZGRluVsm5jGW8Ea1xnNOkn8MoRYEYlFfFTesy5H5sYtyf4qaOfCRmRTIcDzH_Kx0rwJb8hOBPOW5kzB9UhDRYW9qlAVNxx9gkXNoi5pRtiuZRmJdOUWR775jkNi6iPvvVuElb9rCsiJRmb1csFfXzKO8DClld3QvAIhAefuKcUGzc34DfIUMhUS_nYMEmMruJKEo_j58rWqFqv-9HKtKKzNYyGbR8WsCAnBVeOkhP62vc3mJEiwPtsqtp2vJI1cVA7GP-Ez1jvezGA9e5S0FKXELT0irUkq1LAegEPrI5o51Jjzk4bhgsO-lAXXmCrglR3YtrRHThVxPlzu_TSex1XeMjS5lZqUiTZeyi2wHevv0MKbB-70_FOaqUsZdWHZTQUHKMfTqNRDaP7xXNE_iyYapEn1X_vjA7oxEevu8niP4vWTweMl66ci0VuzylWrNK3-d-EAuj6eLnjGwjrQCBV2tM-Vy0), can be edit.

## sequence diagram([click here to see origin SVG image](https://camo.githubusercontent.com/3f1bc38b251129586050a6e70859e97d73f97317/687474703a2f2f7777772e706c616e74756d6c2e636f6d2f706c616e74756d6c2f7376672f624c52544a7a696d34377a455f6567336748387966414e3065456b613351367a4a6645306a6d75587278637335657554734778475f564b78767543635a4c45736c6a4a53782d464554765647362d6b4f534b4b6375766f2d314853764b6d77453351505a536d70516636585773694d757064385871556d3876727332744c673773514a466b6d5a6f32595561644b596e6c434671786337376c6c6d375465396c6b4a5245786249566b6f66596d4c73586443596d4f4c706a4e524c32392d4d4c43524b44784a66434f72396770666d73375638695a666b5539542d7742506633675846452d3050586a386c37374f784e3142684d626c52574541486536564e523668335239794e33776137687a57706338334c42336f5250714a7a3872586454373375396e4d4d48385a494a493734626237313241726c45756856674b4a42365a4876344b2d514f732d5065397a515057686c324f434e6a3368432d7065344564326f354557334e6d70596d334a5570697044466a654c7166515a4857334b5354457669486e6e5359766c507a3665453550496157616461686735436f43427466364b7469325a7a70495664592d6c665a416f76344a634c626d6c2d33386a486d75334855337050627071324169786f785f734c335951756c466d6f672d786a662d5452744d4344515479644533656d2d723575773245646a556e375379645349666a697959684b6e4e6146785a786a524d486d555338615161736372473532532d674e6d43464e4c436f396271314555673770765f74655533702d687178787657412d654463775a4c316865315131566e76684261583136764b324d2d654e5a394c733237476d3051794f75357833315773326945336568543273755466575a326535525f355857566b6c7467684339434569416936387a5352357a4153734d74684e385055437162525a454f393576337a5675553773586e5449636e474e5a364871476d6c6c47754953786c3768646738364d4866374a5a696338725738764e3244766b576e67646f493134653867646e576e482d346b614177754f624769446d776a41546e663938444d503637715961634179436f6836737959795430687932334b38417352415f4e2d54635f7538326d306b4b4d32656a66492d77355a5761766244315a754f497341544959344d5a526f6451623552656c524d654451786c356a5873796a597574423674673351536a56782d3476724432724d33795f596679784e5834715364716a424757486a593135795157456b48484d734a6942556e306d42335172594b696378756c6e4e666d466d5a687446716e5a666c727a5f5f4f41526f3865736550392d4c637059347755755f4b5a506833715f4a6452496a417a48376c77526d4a7a304c4a6a326b62584269647a4f51457a41466e426d3030))

![sequence diagram](http://www.plantuml.com/plantuml/svg/bLRTJzim47zE_eg3gH8yfAN0eEka3Q6zJfE0jmuXrxcs5euTsGxG_VKxvuCcZLEsljJSx-FETvVG6-kOSKKcuvo-1HSvKmwE3QPZSmpQf6XWsiMupd8XqUm8vrs2tLg7sQJFkmZo2YUadKYnlCFqxc77llm7Te9lkJRExbIVkofYmLsXdCYmOLpjNRL29-MLCRKDxJfCOr9gpfms7V8iZfkU9T-wBPf3gXFE-0PXj8l77OxN1BhMblRWEAHe6VNR6h3R9yN3wa7hzWpc83LB3oRPqJz8rXdT73u9nMMH8ZIJI74bb712ArlEuhVgKJB6ZHv4K-QOs-Pe9zQPWhl2OCNj3hC-pe4Ed2o5EW3NmpYm3JUpipDFjeLqfQZHW3KSTEviHnnSYvlPz6eE5PIaWadahg5CoCBtf6Kti2ZzpIVdY-lfZAov4JcLbml-38jHmu3HU3pPbpq2Aixox_sL3YQulFmog-xjf-TRtMCDQTydE3em-r5uw2EdjUn7SydSIfjiyYhKnNaFxZxjRMHmUS8aQascrG52S-gNmCFNLCo9bq1EUg7pv_teU3p-hqxxvWA-eDcwZL1he1Q1VnvhBaX16vK2M-eNZ9Ls27Gm0QyOu5x31Ws2iE3ehT2suTfWZ2e5R_5XWVkltghC9CEiAi68zSR5zASsMthN8PUCqbRZEO95v3zVuU7sXnTIcnGNZ6HqGmllGuISxl7hdg86MHf7JZic8rW8vN2DvkWngdoI14e8gdnWnH-4kaAwuObGiDmwjATnf98DMP67qYacAyCoh6syYyT0hy23K8AsRA_N-Tc_u82m0kKM2ejfI-w5ZWavbD1ZuOIsATIY4MZRodQb5RelRMeDQxl5jXsyjYutB6tg3QSjVx-4vrD2rM3y_YfyxNX4qSdqjBGWHjY15yQWEkHHMsJiBUn0mB3QrYKicxulnNfmFmZhtFqnZflrz__OARo8eseP9-LcpY4wUu_KZPh3q_JdRIjAzH7lwRmJz0LJj2kbXBidzOQEzAFnBm00)


## PlantUML source code

```shell
@startuml
skinparam titleBorderRoundCorner 15
skinparam titleBorderThickness 2
skinparam titleBorderColor red
skinparam titleBackgroundColor Aqua-CadetBlue
title Red compile process and runtime initial\n\nBase on Red 0.6.3(https://github.com/red/red/releases)\n\nSee: http://www.red-lang.org/2011/05/redsystem-compiler-overview.html

skinparam ParticipantPadding 20
skinparam BoxPadding 10

actor Reducer

box "Red command-line front-end" #LightBlue
    participant "red.r\n\nredc: context" as red.r
end box

box "Red compiler"
    participant "compiler.r\n\nred: context" as redcompiler 
end box

box "Red/System compiler" #DarkSalmon
    participant "system/compiler.r\n\nsystem-dialect: context" as rscompiler    
    participant "system/linker.r\n\nlinker: context" as linker
    participant "system/emitter.r\n\nemitter: context" as emitter
    participant "system/loader.r\n\nloader: context" as loader
    participant "system/lexer.r\n\nlexer: context" as lexer
    participant "system/utils/libRedRT.r\n\nlibRedRT: context" as libRedRT.r
end box

box "Red runtime initial" #FFBBBB
    participant "runtime/red.reds\n\nred: context" as redrt
    participant "runtime/allocator.reds" as redalloc
end box

autonumber "<font color=red><b>Step-0  "
Reducer -> red.r : rebol>> do/args %red.r "--release %tests/hello.red"

||45||
== Load compiler toolchain(1): compiler / linker / emitter ==

red.r -> redcompiler : @17> do-cache %compiler.r
redcompiler -> rscompiler : @10> do-cache %system/compiler.r
rscompiler -> linker : @19> do-cache %system/linker.r
linker -[#0000FF]-> rscompiler : return linker: context
rscompiler -> emitter : @20> do-cache %system/emitter.r
emitter -[#0000FF]-> rscompiler : return emitter: context

||45||
== Prepare Red runtime includes ==

rscompiler -> libRedRT.r : @21> do-cache %system/utils/libRedRT.r
libRedRT.r -> libRedRT.r : @13> set [funcs vars] load-cache %system/utils/libRedRT-exports.r\ninclude red/(boot & actions & natives & stack...)
libRedRT.r -[#0000FF]-> rscompiler : return libRedRT: context

||45||
== Load compiler toolchain(2): loader / lexer ==

rscompiler -> loader : @29> loader: do bind load-cache %system/loader.r 'self
loader -> lexer : @10> do-cache %lexer.r
lexer -[#0000FF]-> loader : return lexer: context
loader -[#0000FF]-> rscompiler : return loader: context
rscompiler -[#0000FF]-> redcompiler : return system-dialect: context

||45||
== Initial Red compiler options ==

redcompiler -> redcompiler : load other things
note over redcompiler
	"**Red compiler load other things**"
	lexer: do bind load-cache %lexer.r 'self
	extracts: do bind load-cache %utils/extractor.r 'self
	redbin:	do bind load-cache %utils/redbin.r 'self
	preprocessor: do-cache file: %utils/preprocessor.r
	preprocessor: do preprocessor/expand/clean load-cache file none
end note

redcompiler -[#0000FF]-> red.r : return red: context

||45||
== Compile Red to Red/System ==

red.r -> red.r : @870> redc/main\n@852> if result: compile src opts\n@797> if needs-libRedRT? opts [build-libRedRT opts]
red.r -> redcompiler : @518> result: red/compile script opts
redcompiler -> rscompiler : @4673> if file? file [system-dialect/collect-resources src/1 resources file]
rscompiler -> redcompiler : return %system/assets/red.ico image
redcompiler -> redcompiler : @4691: comp-as-exe src
note over redcompiler
    **`comp-as-exe` return a Red/System code template and its Redbin**
Red/System [origin: 'Red] 
red/init ;**initial Red runtime**
with red [ ;**but WHERE is the `red context?**
    exec: context [
        ------------| "Symbols" 
        ------------| "Literals"
        ------------| "Declarations"
        ------------| "Functions"
        ------------| "Main program"
    ]
]
end note
redcompiler -[#0000FF]-> red.r : return Red/System code template above


||45||
== Load and compile Red runtime, generate a executable ==

red.r -> rscompiler : @821> system-dialect/compile/options/loaded src opts result
rscompiler -> rscompiler : @3906> comp-runtime-prolog to logic! loaded all [loaded job-data/3]
rscompiler -> rscompiler : @3906> script: pick [%red.reds %../runtime/red.reds] encap?
rscompiler -> loader : @3906> script:  job loader/process/own script script
loader -> redrt : load many things Red runtime needs
redrt -> redrt : include many things Red runtime needs
note over redrt 
#include %definitions.reds
#include %macros.reds
#include %platform/win32.reds
#include %allocator.reds
#include %datatypes/structures.reds
#include %datatypes/datatype.reds
#include %actions.reds
#include %natives.reds
#include %stack.reds
#include ...
end note

redrt --> loader : return Red runtime files
loader --> rscompiler : return red: context

rscompiler -> rscompiler : compiler/run job loader/process/own script script
rscompiler -> rscompiler : comp-dialect
note over rscompiler
emit %runtime/common.reds
emit %red.reds
emit %hello.reds
--->
---> In other words, this step will compile, load, emit below Red/System code
Red/System [origin: 'Red] 
red: context [...] ;**Now we have the `red context**
red/init
with red [
    exec: context [
        ------------| "Symbols" 
        ------------| "Literals"
        ------------| "Declarations"
        ------------| "Functions"
        ------------| "Main program"
    ]
]
end note

rscompiler --> red.r : generate a executable hello.exe
red.r --> Reducer : return a executable hello.exe


||45||
== User run the executable hello.exe ==
Reducer -> redrt : run the executable hello.exe
redrt -> redalloc : red/init
redalloc --> redrt : allocate Red runtime memory
note over redalloc
memory: declare struct! [					; TBD: instanciate this structure per OS thread
	total	 [integer!]						;-- total memory size allocated (in bytes)
	n-head	 [node-frame!]					;-- head of node frames list
	n-active [node-frame!]					;-- actively used node frame
	n-tail	 [node-frame!]					;-- tail of node frames list
	s-head	 [series-frame!]				;-- head of series frames list
	s-active [series-frame!]				;-- actively used series frame
	s-tail	 [series-frame!]				;-- tail of series frames list
	s-start	 [integer!]						;-- start size for new series frame		(1)
	s-size	 [integer!]						;-- current size for new series frame	(1)
	s-max	 [integer!]						;-- max size for new series frames		(1)
	b-head	 [big-frame!]					;-- head of big frames list
]

init-mem: does [
	memory/total: 	0
	memory/s-start: _1MB
	memory/s-max: 	_2MB
	memory/s-size: 	memory/s-start
]
end note

@enduml
```