# 把Red编译成Red System代码


## 例子：test.red

```red
Red [ ]

; 数值
a: 1
b: 2 + a
print b

; 字符串
s: "hello"
prin s
print " world"

; 函数定义与调用
inc: func [n][n + 1]
b: inc b
print b

```

## 把 Red 编译成 Red/System

在 `test.red` 所在目录下执行 `red-063.exe -c -v 4 test.red > v4.reds`，得到 `v4.reds` 文件如下。

可以看到 `int` 变量 `a`、`b` 以及函数 `inc` 的符号表（指在 `------------| "Symbols"` 部分），但找不到字符串变量 `s`。

```red
with red [
    root-base: redbin/boot-load system/boot-data yes 
    exec: context [
        ------------| "Symbols" 
        ~datatype!: word/load "datatype!" 
        ~make: word/load "make" 
        ~unset!: word/load "unset!" 
        ~none!: word/load "none!" 
        ~logic!: word/load "logic!" 
        ~block!: word/load "block!" 
        ~string!: word/load "string!" 
        ~integer!: word/load "integer!" 
...省略...
        ~ctx348~fetch-options: word/load "ctx348~fetch-options" 
        ~ctx348~make-actor: word/load "ctx348~make-actor" 
        ~all?: word/load "all?" 

; 下面 3 个是 test.red 中声明的 int 变量和函数，但是没有字符串变量 s
        ~a435: word/load "a" 
        ~b436: word/load "b" 
        ~inc: word/load "inc" 
        ------------| "Literals" 
        ctx45: get-root-node2 89 
        ctx46: get-root-node2 92
...省略...
        ctx431: get-root-node2 1433 
        ctx437: get-root-node 2 
        ts438: as red-typeset! get-root 5 
        ------------| "Declarations" 
        ------------| "Functions" 
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
        ------------| "Main program" ; test.red 的主程序
        stack/mark-native ~set 
        word/push ~datatype! 
        datatype/push TYPE_DATATYPE 
        word/set 
        stack/unwind 
        stack/reset 
        #script %/Volumes/Red/samples/test/test.red 
        #either type = 'exe [stack/push get-cmdline-args] [none/push] 
        #script %/Volumes/Red/samples/test/test.red 
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
        stack/mark-native ~set 
        word/push ~a435 ----->变量 a
        integer/push 1 
        word/set 
        stack/unwind 
        stack/reset ------------| "a: 1" 
        stack/mark-native ~set 
        word/push ~b436 ----->变量 b
        stack/mark-native ~+ 
        integer/push 2 
        word/get ~a435 
        actions/add* 
        stack/unwind 
        word/set 
        stack/unwind 
        stack/reset ------------| "b: 2 + a" 
        stack/mark-native ~print 
        word/get ~b436 
        natives/print* true 
        stack/unwind 
        stack/reset ------------| "print b" 
        stack/mark-native ~set 
        word/push ~s -----------> 字符串变量 s ？
        string/push as red-string! get-root 0 
        word/set 
        stack/unwind 
        stack/reset ------------| {s: "hello"} 
        stack/mark-native ~prin 
        word/get ~s 
        natives/prin* true 
        stack/unwind 
        stack/reset ------------| "prin s" 
        stack/mark-native ~print 
        string/push as red-string! get-root 1 
        natives/print* true 
        stack/unwind 
        stack/reset ------------| {print " world"} 
        stack/mark-native ~set 
        word/push ~inc 
        _function/push get-root 3 get-root 4 ctx437 as integer! :f_inc null 
        word/set 
        stack/unwind 
        stack/reset ------------| "inc: func [n] [n + 1]" 
        stack/mark-native ~set 
        word/push ~b436 
        stack/mark-func ~inc 
        word/get ~b436 
        f_inc 
        stack/unwind 
        word/set 
        stack/unwind 
        stack/reset ------------| "b: inc b" 
        stack/mark-native ~print 
        word/get ~b436 
        natives/print* true 
        stack/unwind ------------| "print b" #user-code
        ]
    ]
]
...compilation time : 47 ms
```

## 字符串变量的符号表在哪里？

字符串变量 `s` 在编译后是用 `~s` 来表示，于是在 `v4.reds` 中搜索 `~s`，发现在 `-- compiler/globals --` 和 `emiter/symbols` 中分别找到了，前者说明字符串是全局的？后者说明是从 Redbin 的符号表来读取？

```red
-- compiler/globals -- 
[
    <data> [global 0 [144 150] -] 
    stdout [global 4 [110] -] 
    stdin [global 8 [95] -] 
    stderr [global 12 [125] -] 
    newline [global 16 [] -] 
    <data> [global 20 [] [17]] 
    lf [global 22 [43601 44310 45131 45166] -] 
    cr [global 23 [] -] 
    tab [global 24 [] -] 
    space [global 25 [] -] 
...省略...
    exec/~active? [red/red-word!] 
    exec/~trace? [red/red-word!] 
    exec/~s [red/red-word!] -------->这个是吗？
    exec/~do-quit [red/red-word!] 
]

-- emitter/symbols -- 
[
    exec/~active? [global 10256 [20252] -] 
    <data> [global 10260 [20266] -] 
    exec/~trace? [global 10268 [20279] -] 
    <data> [global 10272 [20293] -] 
    exec/~s [global 10276 [20306 40837 40982] -] ----> 这是在读取 Redbin？说明字符串是在全局 data 段里？
    <data> [global 10280 [20320] -] 
    exec/~do-quit [global 10288 [20333] -] 
]
```