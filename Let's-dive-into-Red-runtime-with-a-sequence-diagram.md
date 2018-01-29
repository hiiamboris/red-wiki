I make a `sequence diagram` from `do/args -r %red.r ...` to compiler a Red script, using [PlantUML](http://plantuml.com/sequence-diagram), in order to see how Red runtime starts.

Here is the PlantUML source, it must exists many mistakes. Wish you could correct my mistakes, thanks!

You can change the source below and copy to [PlantUML online server](http://www.plantuml.com/plantuml/uml) to regenerate a new diagram.

Now I know the Red runtime is stars from [runtime/red.reds](https://github.com/red/red/blob/master/runtime/red.reds#L139), and it's memory model is here [runtime/allocator.reds](https://github.com/red/red/blob/master/runtime/allocator.reds#L84).

Go Red!

## PlantUML source code link

[link](http://www.plantuml.com/plantuml/uml/bLRTJzim47zE_eg3gH8yfAN0eEka3Q6zJfE0jmuXrxcs5euTsGxG_VKxvuCcZLEsljJSx-FETvVG6-kOSKKcuvo-1HSvKmwE3QPZSmpQf6XWsiMupd8XqUm8vrs2tLg7sQJFkmZo2YUadKYnlCFqxc77llm7Te9lkJRExbIVkofYmLsXdCYmOLpjNRL29-MLCRKDxJfCOr9gpfms7V8iZfkU9T-wBPf3gXFE-0PXj8l77OxN1BhMblRWEAHe6VNR6h3R9yN3wa7hzWpc83LB3oRPqJz8rXdT73u9nMMH8ZIJI74bb712ArlEuhVgKJB6ZHv4K-QOs-Pe9zQPWhl2OCNj3hC-pe4Ed2o5EW3NmpYm3JUpipDFjeLqfQZHW3KSTEviHnnSYvlPz6eE5PIaWadahg5CoCBtf6Kti2ZzpIVdY-lfZAov4JcLbml-38jHmu3HU3pPbpq2Aixox_sL3YQulFmog-xjf-TRtMCDQTydE3em-r5uw2EdjUn7SydSIfjiyYhKnNaFxZxjRMHmUS8aQascrG52S-gNmCFNLCo9bq1EUg7pv_teU3p-hqxxvWA-eDcwZL1he1Q1VnvhBaX16vK2M-eNZ9Ls27Gm0QyOu5x31Ws2iE3ehT2suTfWZ2e5R_5XWVkltghC9CEiAi68zSR5zASsMthN8PUCqbRZEO95v3zVuU7sXnTIcnGNZ6HqGmllGuISxl7hdg86MHf7JZic8rW8vN2DvkWngdoI14e8gdnWnH-4kaAwuObGiDmwjATnf98DMP67qYacAyCoh6syYyT0hy23K8AsRA_N-Tc_u82m0kKM2ejfI-w5ZWavbD1ZuOIsATIY4MZRodQb5RelRMeDQxl5jXsyjYutB6tg3QSjVx-4vrD2rM3y_YfyxNX4qSdqjBGWHjY15yQWEkHHMsJiBUn0mB3QrYKicxulnNfmFmZhtFqnZflrz__OARo8eseP9-LcpY4wUu_KZPh3q_JdRIjAzH7lwRmJz0LJj2kbXBidzOQEzAFnBm00), can be edit.

## sequence diagram([click here to see origin SVG image](https://camo.githubusercontent.com/3f1bc38b251129586050a6e70859e97d73f97317/687474703a2f2f7777772e706c616e74756d6c2e636f6d2f706c616e74756d6c2f7376672f624c52544a7a696d34377a455f6567336748387966414e3065456b613351367a4a6645306a6d75587278637335657554734778475f564b78767543635a4c45736c6a4a53782d464554765647362d6b4f534b4b6375766f2d314853764b6d77453351505a536d70516636585773694d757064385871556d3876727332744c673773514a466b6d5a6f32595561644b596e6c434671786337376c6c6d375465396c6b4a5245786249566b6f66596d4c73586443596d4f4c706a4e524c32392d4d4c43524b44784a66434f72396770666d73375638695a666b5539542d7742506633675846452d3050586a386c37374f784e3142684d626c52574541486536564e523668335239794e33776137687a57706338334c42336f5250714a7a3872586454373375396e4d4d48385a494a493734626237313241726c45756856674b4a42365a4876344b2d514f732d5065397a515057686c324f434e6a3368432d7065344564326f354557334e6d70596d334a5570697044466a654c7166515a4857334b5354457669486e6e5359766c507a3665453550496157616461686735436f43427466364b7469325a7a70495664592d6c665a416f76344a634c626d6c2d33386a486d75334855337050627071324169786f785f734c335951756c466d6f672d786a662d5452744d4344515479644533656d2d723575773245646a556e375379645349666a697959684b6e4e6146785a786a524d486d555338615161736372473532532d674e6d43464e4c436f396271314555673770765f74655533702d687178787657412d654463775a4c316865315131566e76684261583136764b324d2d654e5a394c733237476d3051794f75357833315773326945336568543273755466575a326535525f355857566b6c7467684339434569416936387a5352357a4153734d74684e385055437162525a454f393576337a5675553773586e5449636e474e5a364871476d6c6c47754953786c3768646738364d4866374a5a696338725738764e3244766b576e67646f493134653867646e576e482d346b614177754f624769446d776a41546e663938444d503637715961634179436f6836737959795430687932334b38417352415f4e2d54635f7538326d306b4b4d32656a66492d77355a5761766244315a754f497341544959344d5a526f6451623552656c524d654451786c356a5873796a597574423674673351536a56782d3476724432724d33795f596679784e5834715364716a424757486a593135795157456b48484d734a6942556e306d42335172594b696378756c6e4e666d466d5a687446716e5a666c727a5f5f4f41526f3865736550392d4c637059347755755f4b5a506833715f4a6452496a417a48376c77526d4a7a304c4a6a326b62584269647a4f51457a41466e426d3030))

![sequence diagram](http://www.plantuml.com/plantuml/svg/bLRTJzim47zE_eg3gH8yfAN0eEka3Q6zJfE0jmuXrxcs5euTsGxG_VKxvuCcZLEsljJSx-FETvVG6-kOSKKcuvo-1HSvKmwE3QPZSmpQf6XWsiMupd8XqUm8vrs2tLg7sQJFkmZo2YUadKYnlCFqxc77llm7Te9lkJRExbIVkofYmLsXdCYmOLpjNRL29-MLCRKDxJfCOr9gpfms7V8iZfkU9T-wBPf3gXFE-0PXj8l77OxN1BhMblRWEAHe6VNR6h3R9yN3wa7hzWpc83LB3oRPqJz8rXdT73u9nMMH8ZIJI74bb712ArlEuhVgKJB6ZHv4K-QOs-Pe9zQPWhl2OCNj3hC-pe4Ed2o5EW3NmpYm3JUpipDFjeLqfQZHW3KSTEviHnnSYvlPz6eE5PIaWadahg5CoCBtf6Kti2ZzpIVdY-lfZAov4JcLbml-38jHmu3HU3pPbpq2Aixox_sL3YQulFmog-xjf-TRtMCDQTydE3em-r5uw2EdjUn7SydSIfjiyYhKnNaFxZxjRMHmUS8aQascrG52S-gNmCFNLCo9bq1EUg7pv_teU3p-hqxxvWA-eDcwZL1he1Q1VnvhBaX16vK2M-eNZ9Ls27Gm0QyOu5x31Ws2iE3ehT2suTfWZ2e5R_5XWVkltghC9CEiAi68zSR5zASsMthN8PUCqbRZEO95v3zVuU7sXnTIcnGNZ6HqGmllGuISxl7hdg86MHf7JZic8rW8vN2DvkWngdoI14e8gdnWnH-4kaAwuObGiDmwjATnf98DMP67qYacAyCoh6syYyT0hy23K8AsRA_N-Tc_u82m0kKM2ejfI-w5ZWavbD1ZuOIsATIY4MZRodQb5RelRMeDQxl5jXsyjYutB6tg3QSjVx-4vrD2rM3y_YfyxNX4qSdqjBGWHjY15yQWEkHHMsJiBUn0mB3QrYKicxulnNfmFmZhtFqnZflrz__OARo8eseP9-LcpY4wUu_KZPh3q_JdRIjAzH7lwRmJz0LJj2kbXBidzOQEzAFnBm00)


## PlantUML source code

```shell
@startuml
participant "red.r" order 1
participant "compiler.r" order 2
participant "system/compiler.r" order 3
participant "system/utils/libRedRT.r" order 4
participant "system/utils/libRedRT-exports.r" order 5
participant "Red Runtime" order 6
participant "runtime/red.reds" order 7
participant "runtime/allocator.reds" order 8

"red.r" -> "compiler.r" : do-cache %compiler.r
"compiler.r" -> "system/compiler.r" : do-cache %system/compiler.r
"system/compiler.r" -> "system/utils/libRedRT.r" : do-cache %system/utils/libRedRT.r
"system/utils/libRedRT.r" -> "system/utils/libRedRT-exports.r" : load-cache %system/utils/libRedRT-exports.r
"system/utils/libRedRT.r" -> "Red Runtime" : #include runtime/definitions.reds\n#include runtime/macros.reds\n#include runtime/datatypes/structures.reds

"red.r" -> "red.r" : redc/main
"red.r" -> "red.r" : redc/compile
"red.r" -> "compiler.r" : @800: result: red/compile src opts
"red.r" -> "system/compiler.r" : @816: system-dialect/compile/options src opts
"system/compiler.r" -> "runtime/red.reds" : @3718: script: pick [%red.reds %../runtime/red.reds] encap?
"compiler.r" -> "runtime/red.reds" : @4498: red/init

"runtime/red.reds" -> "runtime/red.reds" : #include %definitions.reds\n#include %macros.reds\n#include %tools.reds\n#include %platform/win32.reds\n#include %allocator.reds\n#include %datatypes/structures.reds\n#include %datatypes/common.reds\n#include %datatypes/datatype.reds\n#include %actions.reds\n#include %natives.reds\n#include %stack.reds\n#include ...

"runtime/red.reds" -> "runtime/red.reds" : init everything for Red runtime listed above
note over "runtime/red.reds"
  So here is the Red runtime core
end note

"runtime/red.reds" -> "runtime/allocator.reds" : init-mem
note right
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