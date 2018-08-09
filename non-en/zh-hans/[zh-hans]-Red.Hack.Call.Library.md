* 1. 增加符号序列@runtime/macros.reds 第241行 插入以下内容
```red
	; add by tomac
	NAT_LIBOPEN
	NAT_LIBCLOSE
	NAT_LIBCALL
	; end by tomac

```


* 2. 增加Red中的Word声明@environment/natives.red  第860行即文件尾部插入以下内容
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
		handle [integer!]
		func   [string!]
		vars [block!]
		return: 			[integer!] 
	]
	#get-definition NAT_LIBCALL
]
```

* 3. 增加到词汇表 @ runtime/natives.reds 行 2853 插入以下内容
```red
			; add by tomac
			:libopen*
			:libclose*
			:libcall*
			; end add by tomac
```


* 4. 增加Red/System的功能实现 @ runtime/natives.reds 尾部的!!!!中括号前!!!!插入以下内容
```red
	; add by tomac
	libopen*: func [ 
		check?			[logic!]
		return: 		[red-integer!] 
		/local 
			libname 	[red-string!]  
			cname 		[c-string!]
			r			[red-integer!]
			h			[integer!]
	][
		;#typecheck libopen
		; 从栈顶取得第一个参数
		libname: as red-string! stack/arguments
		; 从red字串转换成char* 即 c-string!
		cname: as c-string! string/rs-head libname 
		h: _dlopen cname 0 
		r: as red-integer! libname
		r/value: h
		r/header: TYPE_INTEGER
		stack/set-last r 
	]


	libclose*: func[ 
		check?			[logic!]
		return: 		[red-integer!] 
		/local 
			handle 		[red-integer!]  
			cname 		[c-string!]
			h			[integer!]
	][
		; 取出栈顶参数
		handle: as red-integer! stack/arguments
		;执行
		h: _dlclose handle/value ; system/runtime/dl.reds
		;利用输入的内存作返回
		handle/value: h
		stack/set-last handle
	]


	; 没有参数的调用类型
	libfunc0!: alias function! [ return: [integer!]]
	; 一个参数
	libfunc1!: alias function! [ a0 [integer!] return: [integer!]]
	libfunc2!: alias function! [ a0 [integer!] a1 [integer!] return: [integer!]]
	libfunc3!: alias function! [ a0 [integer!] a1 [integer!] a2 [integer!] return: [integer!]]
	libfunc4!: alias function! [ a0 [integer!] a1 [integer!] a2 [integer!] a3 [integer!] return: [integer!]]
	; 五个参数
	libfunc5!: alias function! [ a0 [integer!] a1 [integer!] a2 [integer!] a3 [integer!] a4 [integer!] return: [integer!]]
	libfunc6!: alias function! [ a0 [integer!] a1 [integer!] a2 [integer!] a3 [integer!] a4 [integer!] a5 [integer!] return: [integer!]]
	libfunc7!: alias function! [ a0 [integer!] a1 [integer!] a2 [integer!] a3 [integer!] a4 [integer!] a5 [integer!] a6 [integer!] return: [integer!]]
	libfunc8!: alias function! [ a0 [integer!] a1 [integer!] a2 [integer!] a3 [integer!] a4 [integer!] a5 [integer!] a6 [integer!] a7 [integer!] return: [integer!]]

	; libcall handler "function_name" [ param0 param1 parm2 ]
	; libcall handler "function_name" [ ] 
	libcall*: func[ 
		check?			[logic!]
		return: 		[red-integer!] 
		/local
			;输入的 
			handle 		[red-integer!]
			funcstr		[red-string!]
			param		[red-block!]
			;输出的
			r			[red-integer!]
			;转换的
			funcname	[c-string!]
			funcpointer	[integer!]
			;转换后的参数列表
			xparam
			;函数指针
			xfunc0		[libfunc0!]
			xfunc1		[libfunc1!]
			xfunc2		[libfunc2!]
			xfunc3		[libfunc3!]
			xfunc4		[libfunc4!]
			xfunc5		[libfunc5!]
			xfunc6		[libfunc6!]
			xfunc7		[libfunc7!]
			xfunc8		[libfunc8!]
			;其他变量
			value		[red-value!]
			tail		[red-value!]
			count		[integer!]
			wparam		[red-word!]
			sparam		[red-string!]
			iparam		[red-integer!]
			uparam		[red-datatype!]
			vparam		[red-value!]
		
		] 
	[
		xparam: [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 ]
		; 取得输入参数
		handle: as red-integer! stack/arguments
		funcstr: as red-string! stack/arguments + 1 
		param: as red-block! stack/arguments + 2
		; 取得函数指针
		funcname: as c-string! string/rs-head funcstr 
		funcpointer: _dlsym handle/value funcname 

		; 计算参数个数
		value: block/rs-head param		;首个参数
		tail:  block/rs-tail param		;末尾参数
		count: 0 
		while [ value < tail ][
			; 现在还不确定参数类型
			uparam: as red-datatype! value
			case [
				uparam/header = TYPE_INTEGER [
					iparam: as red-integer! value
					xparam/count: iparam/value
					;print [ "integer " iparam/value lf ]
				]
				uparam/header = TYPE_STRING [
					sparam: as red-string! value
					funcname: as c-string! string/rs-head sparam
					xparam/count: as integer! funcname
					;print [ "string " funcname lf ]
				]
				uparam/header = TYPE_WORD [
					wparam: as red-word! value
					vparam: word/get wparam
					case [
						vparam/header = TYPE_INTEGER [
							iparam: as red-integer! vparam
							xparam/count: iparam/value
							;print [ "word integer " iparam/value lf ]
						]
						vparam/header = TYPE_STRING [
							sparam: as red-string! vparam
							funcname: as c-string! string/rs-head sparam
							xparam/count: as integer! funcname
							;print [ "word string " funcname lf ]
						]
						true [
							print [ "word known " lf ]
							count: count - 1 
						]
					]
				]
				true [
					print [ "unknown type = " uparam/header lf ]
					count: count - 1 

				]
			]
			value: value + 1 
			count: count + 1 
		]

		;print [ "libcall param count=" count lf ]

		; 返回值初始化
		r: as red-integer! handle
		r/header: TYPE_INTEGER
		r/value: -1

		case [
			count = 0 [ 
				xfunc0: as libfunc0! funcpointer 
				r/value: xfunc0
			]
			count = 1 [ 
				xfunc1: as libfunc1! funcpointer 
				r/value: xfunc1 xparam/0
			]
			count = 2 [ 
				xfunc2: as libfunc2! funcpointer 
				r/value: xfunc2 xparam/0 xparam/1 
			]
			count = 3 [ 
				xfunc3: as libfunc3! funcpointer 
				r/value: xfunc3 xparam/0 xparam/1 xparam/2 
			]
			count = 4 [ 
				xfunc4: as libfunc4! funcpointer
				r/value: xfunc4 xparam/0 xparam/1 xparam/2 xparam/3
			]
			count = 5 [ 
				xfunc5: as libfunc5! funcpointer
				r/value: xfunc5 xparam/0 xparam/1 xparam/2 xparam/3 xparam/4
			]
			count = 6 [ 
				xfunc6: as libfunc6! funcpointer
				r/value: xfunc6 xparam/0 xparam/1 xparam/2 xparam/3 xparam/4 xparam/5
			]
			count = 7 [ 
				xfunc7: as libfunc7! funcpointer
				r/value: xfunc7 xparam/0 xparam/1 xparam/2 xparam/3 xparam/4 xparam/5 xparam/6
			]
			count = 8 [ 
				xfunc8: as libfunc8! funcpointer
				r/value: xfunc8 xparam/0 xparam/1 xparam/2 xparam/3 xparam/4 xparam/5 xparam/6 xparam/7
			]
			true [ 
				print [ "ERROR: libcall only support max 8 params ." lf ]
			]
		] ; end of case 
		stack/set-last r 
	]


	liberror*: func[ return:[red-string!] /local retval [c-string!] str [red-string!] ][
		retval: _dlerror
		str: as red-string! retval
    	str
	]
	; end add by tomac
```


* 5. 增加libdl的导入@system/runtime/libc.reds 尾部插入
```red
#import [
    "libdl.so" stdcall [
		_dlsym:		"dlsym"	[
			handle		[integer!]
			symbol		[c-string!]
			return: 	[integer!]
		]
		_dlclose:	"dlclose" [
			handle		[integer!]
			return: 	[integer!]
		]
		_dlerror:	"dlerror" [
			return: 	[c-string!]
		]
		_dlopen:		"dlopen" [
			filename	[c-string!]
			flag		[integer!]
			return:		[integer!]
		]
	]
];end of import 

```

* 6.使用样例  以下代码保存为  test.red
```red
Red []
handler: -1
handler: libopen "libmysqlclient.so"

print ["libopen=" handler lf]



MYSQL: libcall handler "mysql_init" [ 0 ] 
print ["mysql_init=" MYSQL lf]


MYSQL2: libcall handler "mysql_real_connect" [ MYSQL "localhost" "root" "root" "mysql" 0 0 0 ]
print ["mysql_real_connect=" MYSQL2 lf]


i: libcall handler "mysql_close" [ MYSQL ]
print ["mysql_close=" i lf]


handler: libclose handler
print ["libclose=" handler lf]
```


* 说明1：以上仅为实验性代码,未处理utf8等格式的字符串，实际还需要检测Red是运行在什么的字符集上，才能更稳定地工作。
* 说明2: 要测试以让代码，在64环境中需要安装32位mysql运行库  apt install libmysqlclinet-dev:i386
* 说明3: 可与我具体探讨 QQ:5379823  AUTHOR:tomac
* 说明4: 以上代码基于  red-0.6.1 源码，下载该版本代码解压后，按上面的步骤修改，安装rebol解释器，在red源码目录运行以下指令
```red
rebol %red.r "%environment/console/console.red" 
./console test.red
```

所得的效果为
```red
tomac-HP-ProBook-440-G3 red # ./console test.red
libopen= 162265888         <= dlopen获得的句柄
mysql_init= 162351600      <= mysql_init 产生的内存块
mysql_real_connect= 162351600   <= 连接成功后，与输入同样的MYSQL结构指针
mysql_close= 1                  <=  mysql_close 本身是void 此处返回值无意义
libclose= 0                     <= 关闭运行库成功

```




