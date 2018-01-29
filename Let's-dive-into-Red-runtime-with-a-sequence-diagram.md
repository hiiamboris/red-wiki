I make a `sequence diagram` from `do/args -r %red.r ...` to compiler a Red script, using [PlantUML](http://plantuml.com/sequence-diagram), in order to see how Red runtime starts.

Here is the PlantUML source, it must exists many mistakes. Wish you could correct my mistakes, thanks!

You can change the source below and copy to [PlantUML online server](http://www.plantuml.com/plantuml/uml) to regenerate a new diagram.

Now I know the Red runtime is stars from [runtime/red.reds](https://github.com/red/red/blob/master/runtime/red.reds#L139), and it's memory model is here [runtime/allocator.reds](https://github.com/red/red/blob/master/runtime/allocator.reds#L84).

Go Red!

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

## PlantUML source code link

[link](http://www.plantuml.com/plantuml/uml/bLRTJzim47zE_eg3gH8yfAN0eEka3Q6zJfE0jmuXrxcs5euTsGxG_VKxvuCcZLEsljJSx-FETvVG6-kOSKKcuvo-1HSvKmwE3QPZSmpQf6XWsiMupd8XqUm8vrs2tLg7sQJFkmZo2YUadKYnlCFqxc77llm7Te9lkJRExbIVkofYmLsXdCYmOLpjNRL29-MLCRKDxJfCOr9gpfms7V8iZfkU9T-wBPf3gXFE-0PXj8l77OxN1BhMblRWEAHe6VNR6h3R9yN3wa7hzWpc83LB3oRPqJz8rXdT73u9nMMH8ZIJI74bb712ArlEuhVgKJB6ZHv4K-QOs-Pe9zQPWhl2OCNj3hC-pe4Ed2o5EW3NmpYm3JUpipDFjeLqfQZHW3KSTEviHnnSYvlPz6eE5PIaWadahg5CoCBtf6Kti2ZzpIVdY-lfZAov4JcLbml-38jHmu3HU3pPbpq2Aixox_sL3YQulFmog-xjf-TRtMCDQTydE3em-r5uw2EdjUn7SydSIfjiyYhKnNaFxZxjRMHmUS8aQascrG52S-gNmCFNLCo9bq1EUg7pv_teU3p-hqxxvWA-eDcwZL1he1Q1VnvhBaX16vK2M-eNZ9Ls27Gm0QyOu5x31Ws2iE3ehT2suTfWZ2e5R_5XWVkltghC9CEiAi68zSR5zASsMthN8PUCqbRZEO95v3zVuU7sXnTIcnGNZ6HqGmllGuISxl7hdg86MHf7JZic8rW8vN2DvkWngdoI14e8gdnWnH-4kaAwuObGiDmwjATnf98DMP67qYacAyCoh6syYyT0hy23K8AsRA_N-Tc_u82m0kKM2ejfI-w5ZWavbD1ZuOIsATIY4MZRodQb5RelRMeDQxl5jXsyjYutB6tg3QSjVx-4vrD2rM3y_YfyxNX4qSdqjBGWHjY15yQWEkHHMsJiBUn0mB3QrYKicxulnNfmFmZhtFqnZflrz__OARo8eseP9-LcpY4wUu_KZPh3q_JdRIjAzH7lwRmJz0LJj2kbXBidzOQEzAFnBm00), can be edit.

## sequence diagram

![sequence diagram](http://www.plantuml.com/plantuml/svg/bLRTJzim47zE_eg3gH8yfAN0eEka3Q6zJfE0jmuXrxcs5euTsGxG_VKxvuCcZLEsljJSx-FETvVG6-kOSKKcuvo-1HSvKmwE3QPZSmpQf6XWsiMupd8XqUm8vrs2tLg7sQJFkmZo2YUadKYnlCFqxc77llm7Te9lkJRExbIVkofYmLsXdCYmOLpjNRL29-MLCRKDxJfCOr9gpfms7V8iZfkU9T-wBPf3gXFE-0PXj8l77OxN1BhMblRWEAHe6VNR6h3R9yN3wa7hzWpc83LB3oRPqJz8rXdT73u9nMMH8ZIJI74bb712ArlEuhVgKJB6ZHv4K-QOs-Pe9zQPWhl2OCNj3hC-pe4Ed2o5EW3NmpYm3JUpipDFjeLqfQZHW3KSTEviHnnSYvlPz6eE5PIaWadahg5CoCBtf6Kti2ZzpIVdY-lfZAov4JcLbml-38jHmu3HU3pPbpq2Aixox_sL3YQulFmog-xjf-TRtMCDQTydE3em-r5uw2EdjUn7SydSIfjiyYhKnNaFxZxjRMHmUS8aQascrG52S-gNmCFNLCo9bq1EUg7pv_teU3p-hqxxvWA-eDcwZL1he1Q1VnvhBaX16vK2M-eNZ9Ls27Gm0QyOu5x31Ws2iE3ehT2suTfWZ2e5R_5XWVkltghC9CEiAi68zSR5zASsMthN8PUCqbRZEO95v3zVuU7sXnTIcnGNZ6HqGmllGuISxl7hdg86MHf7JZic8rW8vN2DvkWngdoI14e8gdnWnH-4kaAwuObGiDmwjATnf98DMP67qYacAyCoh6syYyT0hy23K8AsRA_N-Tc_u82m0kKM2ejfI-w5ZWavbD1ZuOIsATIY4MZRodQb5RelRMeDQxl5jXsyjYutB6tg3QSjVx-4vrD2rM3y_YfyxNX4qSdqjBGWHjY15yQWEkHHMsJiBUn0mB3QrYKicxulnNfmFmZhtFqnZflrz__OARo8eseP9-LcpY4wUu_KZPh3q_JdRIjAzH7lwRmJz0LJj2kbXBidzOQEzAFnBm00)