## 从Red编译成Red System能学到什么

准备好最简单的 Red 源码如下：

```shell
➜ /Users/test >cat test.red
Red [ ]

a: 1
b: 2 + a
print b

s: "hello"
prin s
print " world"

inc: func [n][n + 1]

b: inc b
print b

# 当前目录只有一个 red 源文件
➜ /Users/test >ls -l
test.red
```

用 `--help`查看编译选项，以下 3 个选项是我们要用到的：

```shell
➜ /Users/test >red-063 --help

Usage: red [command] [options] [file]

[file]: any Red or Red/System source file. If no file and no option is provided, the graphical interactive console will be launched. If a file with **no option is provided, the file will be simply run by the interpreter** (it is expected to be a Red script with no Red/System code).

[options]:

    -c, --compile                  : Generate an executable in the working
                                     folder, using libRedRT. (developement mode)

    -v <level>, --verbose <level>  : Set compilation verbosity level, 1-3 for
                                     Red, 4-11 for Red/System.

    --red-only                     : Stop just after Red-level compilation.
                                     Use higher verbose level to see compiler
                                     output. (internal debugging purpose)
```

把 `test.red` 编译成 Red/System 代码，因为编译生成的 Red/System 代码会直接输出到标准输出，我们重定向到 `1.reds` 方便查看。

```shell
➜ /Users/test >red-063 -c -v 1 --red-only test.red > 1.reds

➜ /Users/test >ls -l
1.reds #--> 我们只要看这个，有几万行
libRedRT-defs.r
libRedRT-extras.r
libRedRT-include.red
libRedRT.dylib
test.red
```

查看 `1.reds` 文件：

```shell
... # 省略 2万行，不过建议看一看最前面，都是 #include 必要的文件，可能可以了解到一点东西
...
... # 注意，以下很眼熟，刚好是 test.red 源码中的变量名和函数名，所以这应该是符号表？
... # 但是，下面没有变量 s，也就是没有字符串，很奇怪 -----> 疑问1
~all?: word/load "all?" 
~a435: word/load "a" 
~b436: word/load "b" 
~inc: word/load "inc" 
------------| "Literals" 
ctx45: get-root-node2 89
...
...
...
ts438: as red-typeset! get-root 5 
------------| "Declarations" 
------------| "Functions" # 以下刚好是我们定义的 inc 函数，看函数声明是标准的 Red/System 代码风格
f_inc: func [/local ctx saved ~n] [
    ctx: TO_CTX (ctx437) 
    saved: ctx/values 
    ctx/values: as node! stack/arguments 
    ~n: type-check ts438 0 stack/arguments 
    stack/mark-func-body words/_body 
    stack/reset 
    stack/mark-native ~+ 
    stack/push ~n 
    integer/push 1 
    actions/add* 
    stack/unwind 
    stack/reset ------------| 
    "n + 1" 
    stack/unwind-last 
    ctx/values: saved
] 
------------| "Main program" # 下面就是主函数了吧
stack/mark-native ~set 
word/push ~datatype! 
datatype/push TYPE_DATATYPE 
word/set 
stack/unwind 
stack/reset 
#script %/Users/test/test.red 
#either type = 'exe [stack/push get-cmdline-args] [none/push] 
#script %/Users/test/test.red 
either all [
    object/unchanged? ~system 203 
    object/unchanged2? ctx202 12 244
] [word/set-in-ctx ctx243 4] [
    stack/mark-func ~eval-set-path 
    if stack/arguments > stack/bottom [stack/push stack/arguments - 1] 
    set-path* eval-path _context/get ~system as cell! ~script as cell! ~args 
    stack/unwind
] 
stack/reset ------------| {system/script/args: #system [ #either type = 'exe ...} 
stack/mark-func ~extract-boot-args 
f_extract-boot-args 
stack/unwind ------------| "extract-boot-args" #user-code 
stack/mark-native ~set 			#--> 在栈中构造一个 ~set 设值函数
word/push ~a435 				#--> 取到变量 a 的地址
integer/push 1 					#--> 从 integer 栈中取出数值 1
word/set 						#--> 把 1 赋值给变量 a，以上相当于 ~set a: 1
stack/unwind 
stack/reset ------------| "a: 1" 
stack/mark-native ~set 			#--> 在栈中构造一个 ~set 设值函数
word/push ~b436 				#--> 取到变量 b 的地址
stack/mark-native ~+ 			#--> 加法
integer/push 2 					#--> 从 integer 栈中取出数值 2
word/get ~a435 					#--> 取到变量 b 的地址
actions/add* 					#--> ~set 设值函数优先级低，先算 2 + a
stack/unwind 
word/set 						#--> 再把 2 + a 的结果设值给 b
stack/unwind 
stack/reset ------------| "b: 2 + a" 
stack/mark-native ~print 
word/get ~b436 					#--> 取值 b
natives/print* true 			#--> 准备 print 函数
stack/unwind 					#--> 打印 b
stack/reset ------------| "print b" 
stack/mark-native ~set 
word/push ~s 
string/push as red-string! get-root 0 	#--> 这个 get-root 0 不懂，从某个地方取到字符串 "hello"？
word/set 
stack/unwind 
stack/reset ------------| {s: "hello"} 
stack/mark-native ~prin 
word/get ~s 
natives/prin* true 
stack/unwind 
stack/reset ------------| "prin s" 
stack/mark-native ~print 
string/push as red-string! get-root 1 	#--> 同理 get-root 1 也不懂，把 "world" 入栈，再打印出来
natives/print* true 
stack/unwind 
stack/reset ------------| {print " world"} 
stack/mark-native ~set 
word/push ~inc 
_function/push get-root 3 get-root 4 ctx437 as integer! :f_inc null #--> get-root 3 和 4？
word/set 
stack/unwind 
stack/reset ------------| "inc: func [n] [n + 1]" 
stack/mark-native ~set 
word/push ~b436 
stack/mark-func ~inc 
word/get ~b436 
f_inc 						#--> 这个应该好懂，跟上面一样的，只不过是调用 inc 函数
stack/unwind 
word/set 
stack/unwind 
stack/reset ------------| "b: inc b" 
stack/mark-native ~print 
word/get ~b436 
natives/print* true 
stack/unwind ------------| "print b" #user-code
```



## 困惑

1. 字符串变量 `s: "hello"` 哪里去了？在编译后的  `1.reds` 完全找不到。
2. 字符串字面量 `" world"` 同上。
3. `get-root 1 或 2 或 3 或 4` 是从哪里取值？
4. 为什么要转成这么像汇编入栈出栈的代码？



## 收获

生成的 `1.reds` 后半段，有下面一部分，可以得知 Red 的运行时要先导入以下文件，也就是说 Red 的源头就是这个顺序，可以按这个顺序来阅读 Red runtime 的代码。

```shell
...compilation time : 51 ms
[
    Red/System [origin: 'Red] 
    red: context [
        #include %/Users/hao/test/runtime/definitions.reds 
        #include %/Users/hao/test/runtime/macros.reds 
        #include %/Users/hao/test/runtime/datatypes/structures.reds 
        cell!: alias struct! [
            header [integer!] 
            data1 [integer!] 
            data2 [integer!] 
            data3 [integer!]
        ] 
        series-buffer!: alias struct! [
            flags [integer!] 
            node [int-ptr!] 
            size [integer!] 
            offset [cell!] 
            tail [cell!]
        ] 
        root-base: as cell! 0 
        
        #--> 看到这个 get-root 了，但它是在以 root-base 为起始地址的一块内存中按传入的偏移量来取变量吗？
        get-root: func [
            idx [integer!] 
            return: [red-block!]
        ] [
            as red-block! root-base + idx
        ] 
        get-root-node: func [
            idx [integer!] 
            return: [node!] 
            /local 
            obj [red-object!]
        ] [
            obj: as red-object! get-root idx 
            obj/ctx
        ] 
        
        #--> 可以得知其实 Red 最后是转成 stdcall 的方式来调用函数？
        
        #import [libRedRT-file stdcall [
                boot: "red/boot" [] 
                get-build-date: "red/get-build-date" [return: [c-string!]] 
                copy-cell: "red/copy-cell" [src [cell!] dst [cell!] return: [cell!]] 
                get-root-node2: "red/get-root-node2" [idx [integer!] return: [pointer! [integer!]]] 
                set-int-path*: "red/set-int-path*" [parent [cell!] index [integer!]] 
                eval-int-path*: "red/eval-int-path*" [parent [cell!] index [integer!]] 
                set-path*: "red/set-path*" [parent [cell!] element [cell!]] 
                eval-path*: "red/eval-path*" [parent [cell!] element [cell!]] 
                eval-int-path: "red/eval-int-path" [parent [cell!] index [integer!] return: [cell!]] 
                eval-path: "red/eval-path" [parent [cell!] element [cell!] return: [cell!]] 
                select-key*: "red/select-key*" [sub? [logic!] fetch? [logic!] return: [cell!]] 
                alloc-bytes: "red/alloc-bytes" [size [integer!] return: [pointer! [integer!]]] 
                get-cmdline-args: "red/get-cmdline-args" [return: [cell!]] 
                f_routine: "f_routine" [] 
                f_also: "f_also" [] 
                f_attempt: "f_attempt" [] 
                f_comment: "f_comment" [] 
                f_quit: "f_quit" [] 
```