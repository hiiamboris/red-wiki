Red语言和Red/System之间的交互

* Red语言提供了更多的类型，Red/System只有有限的几种数据类型
* Red/System提供了指针类型，这是Red所没有的
* Red和Red/System交互时，用red-XXX来交互
* 用来交互的类型在  runtime/datatypes/structures.reds 中进行了定义
* 原则：在R/S中使用 red-XXXX 类型来和  Red 中的数据类型相对应
* 问题：不定参数也可以么？ 是的，可以但没有argc变量，可以用列表参数传入


# 常见的数据类型
* 1. string! 在R/S中用 red-string来传送 
```red
red-string!: alias struct! [
	header 	[integer!];-- cell header
	head	[integer!];-- string's head index (zero-based)
	node	[node!]	  ;-- series node pointer
	cache	[c-string!];-- UTF-8 cached version of the string (experimental)
]

myfunc: routine [  
    check? [logic!]
    return: [red-string!]
    /local
        rstr [red-string!]
        cstr [c-string!]

][
    rstr: alias red-string!  stack/arguments  ; 从参数栈中取得首个参数
    cstr: alias c-string! string/rs_head  cstr ; 取得里面的数据
    stack/set-last rstr
]
```
