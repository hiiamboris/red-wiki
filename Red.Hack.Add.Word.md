#为Red增加Word
* 1. runtime/macros.reds 增加一个NAT_XXXX格式的枚举，这个应该是为redbin处理时使用的吧
```
	; add by tomac
	NAT_LIBOPEN
	NAT_LIBCLOSE
	NAT_LIBCALL
	; end by tomac
```

* 2. environment/natives.red  根据其他功能抄一个，改成相应的名字
```
libopen: make native! [[
		"打开共享库"
		libname [string!]
		return: [integer!]			; 如果是零就是出错

	]
	#get-definition NAT_LIBOPEN
]


libclose: make native! [[
		"关闭共享库"
		handle [integer!]
		return: [integer!]			; 
	]
	#get-definition NAT_LIBCLOSE
]


libcall: make native! [[
		"调用共享库"
		handle 		[integer!]
		funcname 	[string!]	
		param 		[block!]
		return: [integer!]
	]
	#get-definition NAT_LIBCALL
]
```

* 3. runtime/nativers.reds  增加用red/system编写的函数，然后注册到表中去
```
```

