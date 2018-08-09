#为Red增加Word
* 1. runtime/macros.reds 增加一个NAT_XXXX格式的枚举，这个应该是为redbin处理时使用的吧
```red
	; add by tomac
	NAT_LIBOPEN
	NAT_LIBCLOSE
	NAT_LIBCALL
	; end by tomac
```

* 2. environment/natives.red  根据其他功能抄一个，改成相应的名字
```red
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
* ３. runtime/nativers.reds 增加red/system的实现函数
```red
	; add by tomac
	libopen*: func [ libname [c-string!]  return: [integer!] /local handle [integer!] ][
		handle: _dlopen libname 0
		handle
	]

	libclose*: func[ handle [integer!] return:[integer!] /local retval [integer!] ][
		retval: _dlclose handle
		retval
	]

	libcall*: func[ 
				[variadic] 
				symbol [c-string!]
				count [integer!] list [int-ptr!] 
				return: [integer!] /local retval [integer!] 
		] 
	[
		handler
		retval: _dlsym handle symbol 
		retval
	]

	liberror*: func[ return:[red-string!] /local retval [c-string!] str [red-string!] ][
		retval: _dlerror
		str: as red-string! retval
    	str
	]
	; end add by tomac

```

* ４. runtime/nativers.reds  注册到表中去
```red
			; add by tomac
			:libopen*
			:libclose*
			:libcall*
			; end add by tomac

```

