# Red/System 语言规范（Red/System Language Specification 中文翻译）

> **Author: Nenad Rakocevic**
>
> **Date: 13/09/2017**
>
> **Revision: 53**
>
> **Status: reference document**
>
> **Home: [red-lang.org](red-lang.org)**
>
> 官方地址：[http://static.red-lang.org/red-system-specs-light.html](http://static.red-lang.org/red-system-specs-light.html)



## 1. 概要（Abstract）

`Red/System` 是 `Red` 编程语言的一种方言（[DSL](http://en.wikipedia.org/wiki/Domain-specific_language)），目的是提供：

- 底层系统编程能力
- 作为构建 `Red` 底层库的工具
- 链接和生成可执行程序的工具

`Red/System` 可以看作等同 C 层级的编程语言，支持内存管理，指针以及非常基础且有限的数据类型。

备注：目前已经提供完整的工具链，可以从 Red/System 源代码生成可执行程序。这是临时状态，因为 `Red/System` 会存在于 Red 中，即被内嵌到 Red 的脚本中。

Implementation note: It is currently provided with a complete tool-chain generating executables from source files. This is a temporary state as Red/System will live inside Red, so will be embedded in Red scripts.



## 2. 语法（Syntax）

`Red/System` 的语法跟 Rebol 语言非常相似，因为目前是用 Rebol 提供的 [LOAD](http://www.rebol.com/docs/words/wload.html) 函数来完成自举阶段的语法解析。Rebol 的语法没有正式的规范和详细的文档，只有一些表面的描述，但这已经足够利用起来。参考：

- [http://www.rebol.com/docs/core23/rebolcore-3.html](http://www.rebol.com/docs/core23/rebolcore-3.html)（这里的表格有个笔误，所有字面量(literal)的引号后面都少写了一个 `^`）
- [http://www.rebol.com/r3/docs/guide/code-syntax.html](http://www.rebol.com/r3/docs/guide/code-syntax.html)

关于 Red 和 Red/System 更完整的语法规范将会在 Red 语言层实现之后再提供。

目前 Red/System 用 8-bit 字符（ASCII）来编码，一旦 Red 语言恰当地支持了 Unicode 之后， `Red/System`  将会切换到用 UTF-8 来编码源文件。

以下是一些实践性的语法讲解。

### 2.1 分隔符（Delimiters）

字符串分隔符：双引号
```rebol
"this is a string"
{This is
  a multiline
  string.
}
```

代码块分隔符：方括号
```red
if a > 0 [print "TRUE"]

either a > 0 [print "TRUE"][print "FALSE"]

while [a > 0][print "loop" a: a - 1]
```

Path 分隔符: 斜杠（表明层级关系）
```red
s: declare struct! [i [integer!] b [byte!]]
s/i: 123
s/b: #"A"
```

### 2.2 格式自由的语法（Free-form syntax）

`Red/System` 和 `Red` 都继承了 Rebol 那种格式自由的语法，唯一的语法约束是在单词之间加空白符，以及正确使用成对的分隔符。

以下是例子都是可以正确运行的：

```red
while [a > 0][print "loop" a: a - 1]

while [a > 0]
    [print "loop" a: a - 1]

while [
   a > 0
][
   print "loop"
   a: a - 1
]
```

暂时还没有代码规范，先遵循标准的 Rebol 实践吧。

### 2.3 注释（Comments）

单行注释：

```red
;this is a commented line

print "hello world"    ; this is another comment
```

多行注释：

```rebol
comment {
    This is a
    multiline
    comment
}
```

使用规则：

- 单行注释可以用于代码的任意地方

- 多行注释也可以用于代码的任何地方，但不能放在表达式中间。例如：

  ```red
  a: 1 + comment {5} 4   ; 这里的 comment {5} 注释会导致编译错误
  ```




## 3. 变量（Variables）

变量是用来表示一个内存地址的标签。这个标签（下文开始叫`标识符`）由一系列可打印的字符组成（不包括空格、换行、制表符等空白字符）。可打印字符是指可以在系统 console 打印出来的字符，ASCII 在 20h-7Eh 之间。但以下这些字符除外，它们被用于分隔符或保留给某些数据类型所用：

```rebol
[ ] { } " ( ) / \ @ # $ % ^ , : ; < >
```

以下字符不能作为变量的首个字符，放在其他位置是可以的：

```rebol
0 1 2 3 4 5 6 7 8 9 '
```

Also there is a another restriction to avoid letting the compiler mistake an hex integer for a variable name. Variable names starting with A-F letters consisting of 2, 4 or 8 A-F and 0-9 characters and ending with h are not allowed.
另一个限制是要避免让编译器把十六进制整型当作变量名。因此变量名不能

所有标识符（变量名和函数名）都是大小写不敏感的（即大写和小写等价）。

> **Unicode 支持（Unicode support）**
>
> 在前面「语法」一节说过目前是自举阶段，还不支持 Unicode，但这在 Red 层是支持的，所以当 Red/System 用 Red 重写后才会支持 Unicode。

### 3.1 设值（Setting a value）

变量可以承载所有可用类型的值。它们可以是真实的值（例如 `integer!` 或 `pointer!`），或者指向真实值的引用（例如 `struct!` 和 `c-string!`）。在变量标识符后面加上冒号 `:` 可以给变量赋值：
```red
foo: 123
bar: "hello"
```

> **多行赋值（Multiple assignments）**
>
> Red/System 目前不支持类似 `a: b: 123` 的多行赋值，这个特性会在未来某个时间点加入，或许是用 Red 重写 Red/System 之后。


### 3.2 取值（Getting a value）

取值或传参给函数时直接使用变量名，不需要任何修饰符。
```red
bar: "hello"
print bar
```
会输出:
```red
hello
```

### 3.3 类型（Typing）

变量必须属于某种类型。变量在使用前不一定要先声明，但必须先初始化。函数内的本地变量必须先声明，本地变量的类型也可以忽略，前提是它在函数内可以被正确初始化。例如下面的例子是可行的用法：
```red
foo: 123
bar: "hello"
size: length? bar
id: GetProcessID                       ;-- 'GetProcessID would return an integer!

compute: func [
   a [integer!]
   return: [integer!]
   /local c                            ;-- 'c 没有声明类型
][
   c: 1                                ;-- c 被推断为 integer! 类型
   a + c
]
```

初始化必须在代码的顶层完成，尝试在代码块中初始化将会导致编译错误。
```red
foo: 123                               ;-- 合法的初始化

if a < b [foo: 123]                    ;-- 非法的初始化
```

> 注意：函数内部也被认为是顶层。

变量可用的类型有：
- integer!
- byte!
- float!
- float32!
- logic!
- c-string!
- struct!
- pointer!




## 4. 数据类型（Datatypes）

下面的数据类型是 Red/System 语言中的[一等公民](http://en.wikipedia.org/wiki/First-class_citizen)。



### 4.1 Integer!

#### 4.1.1 字面量（Literal format）

```red
十进制正整数 :  1234
十进制负整数 :  -1234
十六进制     :  04D2h
```

integer! 类型表示 32 位整型，它所支持的数值范围是 `-2147483648` 至 `2147483647`。

**16进制（Hexadecimal format）**

16进制整数大多数情况用来表示内存地址，或者二进制数据的按位操作。对于字符，所有的 16进制字面量都在源代码的语法分析时被转化位整型数值。所允许的范围是：`00000000h` 至 `FFFFFFFFh`

Hex letters have to be written in **uppercase** and only 2, 4 and 8 characters are allowed (prefixing with leading zeros is allowed).
16进制中的字母必须**大写**，且一共必须有 2、4、8 个字符（前置的 0 不算）。

> **16进制字面量的设计决策（Hex literal form design decision）**
>
> `0x` 前缀经常用来表明是 16进制，这本来也可以用在 Red/System 中，但是由于小写字母 `x` 被用于 Red 的 `pair!` 类型，而 Red/System 是 Red 的一种方言，它必须跟 Red 用相同的方式来表示 16进制数，因此选择了 **<...>h** 来表示 16进制。



### 4.2 Byte!

byte! 类型用于表示 `0-255` 之间的无符号整数。

#### 4.2.1 语法（Syntax）

```red
#"<character>"
#"^<character>"
#"^(hexadecimal)"
#"^(name)"
```
例如：
```red
#"a"
#"A"
#"5"
#"^A"
#"^(1A)"
#"^(back)"
```
详见 [http://www.rebol.com/docs/core23/rebolcore-16.html#section-3.1](http://www.rebol.com/docs/core23/rebolcore-16.html#section-3.1) 获取更多描述。

#### 4.2.2 转型（Casting）

Casting is allowed to some extent (see section "4.9 Type Casting").

```red
foo: as integer! #"a"                  ;-- foo 的值是 97
bar: as byte! foo                      ;-- bar 的值是 #"a"
```

> 注意：试图把大于 255 的整数转成 byte! 会导致数据丢失或数据错误。**（后续版本可以会修改这种情况的处理方式）**

### 4.3 Float!

float! 类型表示了 IEEE-754 的双精度浮点数，它的内存大小是 64 bit。

> 注意：**float64!** 可以等同于 **float!** 来使用。

#### 4.3.1 语法（Syntax）

```red
<sign><digits>.<digits>
```
或用科学计数法：
```red
<sign><digits>E<exponent>
<sign><digits>.<digits>E<exponent>
```
其中：
```red
<sign>     : 可选的 + 或 - 符号
<digits>   : 一个或多个数字
<exponent> : 一个正整数或负整数
```
例如：
```red
0.0
1.0
-12345.6789
3.14159265358979
-1E3
+1.23456E-265
```

float! 字面量的值最大只有 16位，超出部分会被丢掉。

> 关于双精度浮点数的更多信息，请参考 [Wikipedia](http://en.wikipedia.org/wiki/Double-precision_floating-point_format)。

#### 4.3.2 转型（Casting）

可以把 float! 的值转成 float32! 类型，例如：
```red
pi: 3.14159265358979
pi-32: as float32! pi
print pi-32
```
会输出：
```red
3.1415927
```

#### 4.3.3 数学操作（Math operations）

Red/System 支持所有数学操作符（+、-、*、/、//、%），默认对于结果的取整方法遵循就近原则，左右两边的操作数都必须是 float! 类型（不会隐式转换类型）。

对 float! 值进行取模 `//` 和求余 `%` 操作符将会得到一样的结果。



### 4.4 Float32!

float32! 类型表示 IEEE-754 的单精度浮点数，它的内存大小是 32 bit。

> **float32! 的目的（Float32! purpose）**
>
> float32! 这个单精度浮点数类型存在的原因是为了后续和其他流行的库交互而考虑的。纯粹的 Red/System 程序应该把 **float!** 当作默认选择。

#### 4.4.1 语法（Syntax）

float32! 类型没有字面量写法，只能从一个 float! 字面量转型成 float32! 来获取到 float32! 常量。例如：
```red
pi32: as float32! 3.1415927
```
> 关于单精度浮点数的更多信息，请参考 [Wikipedia](http://en.wikipedia.org/wiki/Single-precision_floating-point_format)。

#### 4.4.2 转型（Casting）

It is allowed to apply a type casting transformation on a float32! value to convert it to a float! value. Type casting to integer! is also allowed mainly for bits manipulation purpose (this is not a float32! number to integer! number conversion).

可以把 float32! 转换成 float! 类型的值。

例如：

```red
s: as float32! 3.1415927
print [
   as float! s lf
   as integer! s
]
```
会输出：
```red
3.14159270000000
1518260631
```

#### 4.4.3 数学操作（Math operations）

Red/System 支持所有数学操作符（+、-、*、/、//、%），默认对于结果的取整方法遵循就近原则，左右两边的操作数都必须是 float32! 类型（不会隐式转换类型）。

对 float32! 值进行取模 `//` 和求余 `%` 操作符将会得到一样的结果。



### 4.5 Logic!

The logic! datatype represents boolean values: **TRUE** and **FALSE**. Logic variables are initialized using a literal logic value or the result of a conditional expression.

logic! 类型表示布尔值：**TRUE** 和 **FALSE**。logic! 变量用布尔字面量或条件表达式的结果来初始化。

作为第一公民的数据类型，你可以把 logic! 值或变量作为函数参数或函数返回值。

#### 4.5.1 字面量格式（Literal format）

```red
true
false
```

用字面量初始化 logic! 变量：

```red
foo: true
either foo [print "true"][print "false"]
```

会输出：

```red
true
```

用条件表达式来初始化 logic! 变量：

```red
bar: 2 > 5
either bar [print "true"][print "false"]
```

会输出：

```red
false
```



### 4.6 C-string!

一个 c-string! 值是由一系列非 null 字节末尾跟一个 null 字节构成的。c-string! 变量保存着自身所占用内存第一个字节的地址，所以它可以看作是指向 byte! 类型变量的一个隐式指针。空的 c-string! 变量则在第一个字节保存一个 null 字符。

#### 4.6.1 语法（Syntax）

c-string! 字面量的定义用两个双引号或者用一对大括号括起来：

```red
foo: "I am a c-string"
bar: {I am
  a multiline
  c-string
}
```

> **c-string! 用 null 字节结尾（C-string null byte ending）**
>
> 你不用给 c-string! 字面量末尾加上 null 字节，编译期间会自动加上的。

#### 4.6.2 c-string! 长度（C-string length）

用 `LENGTH?` 函数可以在运行时获取 c-string! 变量的字节数（不包括 null 终结符）。

```red
a: length? "Hello"                     ;-- here length? will return 5
```

> **Length? vs Size?**
>
> 不要混淆 `LENGTH?` 函数和 `SIZE?` 函数， `SIZE?` 函数会返回包括 null 终结符的总字节数。

#### 4.6.3 c-string! 运算（C-string arithmetic）

It is possible to apply some simple math operations on c-string variables like additions and subtractions. C-string address would be increased or decreased by the integer argument.

可以对 c-string! 变量进行一些简单的数学运算，例如加减法。c-string! 的地址将会根据整型参数而增加或减少。

语法：

```red
<c-string> + <n>
<c-string> - <n>

<c-string> : c-string variable
<n>        : expression resulting in an integer! value
```

例子：

```red
s: "hello"                             ;-- let's suppose s points to address 40000000h

s: s + 1                               ;-- now s points to address 40000001h
print s                                ;-- "ello" would be printed
s: s + 1                               ;-- now s points to address 40000002h
print s                                ;-- "llo" would be printed

```

#### 4.6.4 访问字节（Accessing bytes）

可以用 path 方式来访问 c-string! 变量的单个字节：

```red
<c-string>/integer!                    ;-- literal integer index provided
<c-string>/<index>                     ;-- index provided by a variable

<c-string> : a c-string variable
<index>    : an integer variable
```

返回值是 byte! 类型。例如：

```red
foo: "I am a c-string"
foo/1  =>  #"I"                        ;-- byte! value (73)
foo/2  =>  #" "                        ;-- byte! value (32)
...
foo/15 => #"g"                         ;-- byte! value (103)
foo/16 => #"^(00)"                     ;-- byte! value (0) (end marker)
```

> **重要提醒（Important notes）**
>
> 对比 C 语言，Red/System 中的下标是从 1 开始的。用超过 c-string! 长度的下标来访问的行为目前是未定义，最好避开这么做。

用变量作为下标的例子：

```red
c: 4
foo/c  => #"m"                         ;-- byte! value (109)
```

遍历一个 c-string! 变量的简单实现：

```red
foo: "I am a c-string"
bar: foo

until [
     print bar/1
     bar: bar + 1
     bar/1 = null-byte
]
```

会输出：

```red
I am a c-string
```

类似的，也可以用 path 方式（以分号结尾）来修改 c-string! 变量中的字节：

```red
<c-string>/integer!:   <value>         ;-- literal integer index provided
<c-string>/<index>:    <value>         ;-- index provided by a variable

<c-string> : a c-string variable
<index>    : an integer variable
<value>    : a byte! value
```

例如：

```red
foo: "I am a c-string"
foo/3: #"-"
c: 4
foo/c: #"-"
print foo
```

会输出：

```red
I -- a c-string
```



### 4.7 Struct!

`struct!` 类型跟 C 语言中的 `struct` 非常像。它记录了一个或多个值，每个值由自己所属的数据类型。一个 struct! 变量保存了这个 struct! 值在内存中的地址。

> **实现要点（Implementation noe）**
>
> struct! 值的成员会在内存中被填充，以便为不同的目标机器码保留对齐优化的情况（例如在 IA32 架构中会对齐到 4 字节）。`Size?` 函数会返回 struct! 在内存中被填充后的大小。

#### 4.7.1 声明（Declaration）

struct! 的声明用 `DECLARE STRUCT!` 序列跟着一个特殊的 block，这个 block 定义了 struct! 值的成员，每个成员是一对名称和数据类型的定义。

```red
declare struct! [
   <member> [<datatype>]
   ...
]
<member>   : a valid identifier
<datatype> : integer! | byte! | pointer! [integer! | byte!] | logic! |
             float! | float32! | c-string! | struct! [<members>] |
             struct! [<members>] value | function! [<spec>]
```

`DECLARE STRUCT!` 的返回值是这个刚创建的 struct! 值的内存地址。

The returned value of DECLARE STRUCT! is the memory address of the newly created struct! value.

> 注意：当一个 struct! 声明时，它的所有成员都被初始化为 0。

#### 4.7.2 用法（Usage）

```red
s: declare struct! [
   a   [integer!]
   b   [c-string!]
   c   [struct! [d [integer!] e [float!]]]
]

```

上面的例子中所定义的 struct! 值有 3 个成员：a、b、c，每一个都是不同的数据类型。成员 c 是一个 struct! 值的指针，它必须呗赋值一个 struct! 值之后才能使用。因此成员 c 的正确初始化方式是：

```red
s/c: declare struct! [d [integer!]]
```

可以嵌套 struct! 值（而不是 struct! 值的指针），但必须加上 `value` 关键词在被嵌套的 struct! 类型后面：

```red
s2: declare struct! [
   a   [integer!]
   b   [c-string!]
   c   [struct! [d [integer!] e [float!]] value]
]
```

在上面这个例子中，嵌套的 struct! `c` 成员在父级 struct! s2 分配内存空间时，也占用了相应的空间，struct! s2 变量的大小是 20 个字节，而上一个例子中的 s struct! 变量是 12 个字节。 

struct! 指针和值可以任意互相嵌套。

#### 4.7.3 访问成员（Accessing members）

struct! 的成员访问要用 path 方式，语法是：

```red
<struct>/<member>                      ;-- read access
<struct>/<member>: <value>             ;-- write access

<struct>  : a valid struct variable
<member>  : a valid member identifier in <struct>
<value>   : a value of same datatype as <member>
```

上一个例子：

```red
foo: s/a                               ;-- reading member 'a in struct 's
s/a: 123                               ;-- writing 123 in member 'a in struct 's
s/b: "hello"
bar: s/c/d                             ;-- deep read/write access is also possible
```

> 注意：访问 `function!` 类型的成员指针会导致指针被解引用（dereferencing）。

也可以用 get-path 的方式来访问一个 struct! 中的成员指针：

```red
:<struct>/<member>

<struct>  : a valid struct variable
<member>  : a valid member identifier in <struct>
```

返回的类型总是 `pointer! [integer!]` 。It can be freely type-casted to other pointer types.

```red
p: :s/a
p/value                               ;-- returns the value of s/a
p/value: 456                          ;-- sets a new value in s/a
```

#### 4.7.4 struct! 的运算（Struct arithmetic）

可以对 struct! 变量进行一些简单的数学运算，例如加减法。struct! 的地址会根据 struct! 值的大小乘以整型参数而增加或减少。

语法：　

```red
<struct> + <n>
<struct> - <n>

<struct>   : struct variable
<n>        : integer value
```

例子：　

```red
p: declare struct! [                   ;-- let suppose p = 40000000h
   a [integer!]
   b [pointer! [integer!]]
]                                      ;-- struct memory size would be 8 bytes
p: p + 1                               ;-- now p = 40000008h
```

> 注意：struct! 值的大小是跟目标系统和对齐相关的。在上面的例子中，如果它在 32 bit 操作系统下运行，那么 struct! 是对齐到 4 个字节的。

#### 4.7.5 别名（Aliases）

struct! 值的定义有时会很长，所以在某些场景下 struct! 的定义需要插入到其他 struct! 的定义，或函数的规范 block 中时，最好是用别名的方式引用到一个 struct! 的定义。struct! 的别名也支持自引用。

注意点：

- 别名不是值，它不占用任何内存空间，可以把别名看作一种`虚拟的数据类型`。简单来说，别名必须以感叹号 `!`来结尾，以便从源代码中把别名从变量中区分出来。
- 别名只存在于他们自己的命名空间，所以它们不能干预变量名称。

别名的语法：

```red
<name>: alias struct! [
   <member> [<datatype>]
   ...
]
<name>     : the name to use as alias
<member>   : a valid identifier
<datatype> : integer! | byte! | pointer! [integer! | byte!] | logic! |
             float! | float32! | c-string! | struct! [<members>] |
             struct! [<members>] value | function! [<spec>]
```

struct! 值用别名的定义来声明：

```red
<variable>: declare <alias>

<variable>  : a struct variable
<alias>     : a previously declared alias name
```

struct! 的用法示例：

```red
book!: alias struct! [                 ;-- 定义一个新的别名，注意别名用感叹号 ! 结尾
   title       [c-string!]
   author      [c-string!]
   year        [integer!]
   last-book   [book!]                 ;-- 别名自引用
]

gift: declare struct! [
   first  [book!]                      ;-- 引用 book!
   second [book!]                      ;-- 引用 book!
]

gift/first: declare book!              ;-- book! struct allocation

gift/first/title:  "Contact"
gift/first/author: "Carl Sagan"
gift/first/year:   1985

gift2: declare struct! [
   first  [book! value]                ;-- inlined book! struct value
   second [book! value]                ;-- inlined book! struct value
]
```



### 4.8 Pointer!

`pointer!` 类型（后称指针类型）用于保存其他值的内存地址。一个指针的值是由其所指向的值的内存地址和数据类型决定的。而 `c-string!` 和 `struct!` 本身已经是隐式的指针类型，那么剩下可以指向的数据类型只有 `integer!`、`float!`、`float32!` 和 `byte!` 类型(`logic!` 不需要用指针）。

`byte!` 指针等价于 `c-string!` 的引用，不同仅在于对所指向值的表示。`byte!` 指针表示指向一个无固定边界的字节流，而 `c-string!` 指针是指向一个以 null 字节结束的字节数组。

> 实现要点：32位系统下指针的大小是 4 字节，64位系统下是 8 字节。

#### 4.8.1 字面量（Literal format）

用下面的语法来新建指针：

```red
declare pointer! [<datatype>]

<datatype>: integer! | byte! | float! | float32!
```

> **可能的语法糖（Possible syntactic sugar）**
> 本文档的上个版本所用的 `&` 符号已经被移除，是因为 `pointer!` 类型的用法有新的限制。如果有需要，以后可以再引入。

例子：

```red
foo: declare pointer! [integer!]       ;-- 等同于 C 语言的: int *foo;
bar: declare pointer! [byte!]          ;-- 等同于 C 语言的: char *bar;
baz: declare pointer! [float!]         ;-- 等同于 C 语言的: double *baz;
```

> **指针的初始化（Pointer value initialization）**
>
> 在指针被正确初始化之前，不能假设它有任何默认值。在当前的实现中，全局的指针变量默认被设置为 `null`，而本地指针变量的默认值是未定义的。这可能会在将来改成接受一个默认值来方便调试（例如 `0BADBAD0h` 或其他类似的 16进制小技巧）。

#### 4.8.2 声明（Declaration）

指针的声明只在函数的 specification block 里才是必须的。对于本地的指针变量，它的数据类型声明可以忽略掉，留待后面的推断（详见「类型推断」一节）。

```red
pointer! [<datatype>]

<datatype>: integer! | byte! | float! | float32!
```

全局变量声明的例子（及其等价的 C 语言写法）：

```red
p: declare pointer! [integer!]         ;-- int *p;
p: declare pointer! [byte!]            ;-- char *p;
p: declare pointer! [float!]           ;-- double *p;
```

本地变量也是同理：

```red
func [/local p [pointer! [integer!]]   ;-- int *p;
func [/local p [pointer! [byte!]]      ;-- char *p;
func [/local p [pointer! [float!]]     ;-- double *p;
```

指针变量的类型推断：

```red
foo: func [
   a [struct! [count [integer!]]]
   /local
       p [pointer! [integer!]]         ;-- 显式的声明
][
   foobar p                            ;-- foobar 修改 p 指针
   a/count: p/value
]

bar: func [
   a [struct! [count [integer!]]]
   /local p                            ;-- p 将会被推断类型
][
   p: declare pointer! [integer!]      ;-- p 初始化（隐式声明）
   foobar p
   a/count: p/value
]

bar2: func [
   a [struct! [count [integer!]]]
   /local p                            ;-- p 将会被推断类型
][
   p: GetPointer a                     ;-- p 的类型将根据 GetPointer 返回值的类型来推断
   foobar p
   a/count: p/value
]
```

#### 4.8.3 解引用（Dereferencing）

解引用可以访问指针所指的值。在 Red/System 中，它是通过给指针变量增加一个 **value** 修饰词（refinement）来实现的，通常也叫作 path 记法）。

```red
<pointer>/value                        ;-- 获取值
<pointer>/value: <value>               ;-- 写入值

<pointer> : pointer variable
<value>   : 指针定义所指向的相同类型的值
```

用法示例：

```red
p:   declare pointer! [integer!]       ;-- declare a pointer on an integer
bar: declare pointer! [integer!]       ;-- declare another pointer on an integer

p: as [pointer! [integer!]] 40000000h  ;-- type cast an integer! to a pointer!
p/value: 1234                          ;-- write 1234 at address 40000000h
foo: p/value                           ;-- read pointed value back
bar: p                                 ;-- assign pointer address to 'bar
```

> 注意：在你你显式定义指针的值之前，指针的值是未定义的（可能包含任意字符）。

#### 4.8.4 指针运算（Pointer arithmetic）

指针可以进行一些简单的数学运算，例如加减法（跟 C 语言一样）。指针的地址会随着所指向值的内存大小相应地乘以加减的值而增加或减少。

```red
p: declare pointer! [integer!]         ;-- pointed value memory size is 4 bytes

p: as [pointer! [integer!]] 40000000h
p: p + 1                               ;-- p points now to 40000004h
p: p + 1                               ;-- p points now to 40000008h
q: declare pointer! [byte!]            ;-- pointed value memory size is 1 byte
q: as [pointer! [byte!]] 40000000h
q: q + 1                               ;-- p points now to 40000001h
q: q + 1                               ;-- p points now to 40000002h
```

Also, additions and subtractions between pointer addresses are allowed. The result value type is, as usual, the type of left operand.
加减法在指针之间也是允许的。它的返回值一般而言跟左边操作数的类型相同。

```red
offset: p - q                          ;-- would store 6 in offset
                                       ;-- type of offset is pointer! [integer!]
```

#### 4.8.5 指针 path 记法（Pointer path notation）

指针也可以像数组一样用下标来访问，读下都行，下标从 1 开始。

语法：

```red
<pointer>/<integer>                    ;-- literal integer index provided
<pointer>/<index>                      ;-- index provided by a variable

<pointer>  : a pointer variable
<integer>  : an integer literal value
<index>    : an integer variable
```

例子：

```red
p: declare pointer! [integer!]

p: as [pointer! [integer!]] 40000000h
a: p/1                                 ;-- reads an integer! from 40000000h
p/2: a                                 ;-- writes the integer! to 40000004h
```

整型变量也可以作为下标：

```red
p: declare pointer! [integer!]

p: as [pointer! [integer!]] 40000000h
c: 2
p/c: 1234                              ;-- writes 1234 (4 bytes) at 40000004h
```

> 注意：指针的 `/value` 写法严格等价于 `/1`。`/value` 写法可以认为是一个语法糖。

#### 4.8.6 字面量数组（Literal arrays）

指针可以指向有固定字面量的一维数组。

语法：

```red
<variable>: [<items>]

<variable> : 指向数组的指针变量，类型跟数组元素一样
<items>    : 必须是非空的 integer!, byte!, float!, logic! 字面量
```

这个数组是静态分配的，可以用 path 记法来访问，或进行数学运算。数组的大小（指多少个元素）被存放在 32 位的槽中，在数组的起始点之前（即下标 0 的位置）。允许在同一个数组中混合不同的数据类型（枚举也行）。在这种情况下，数组中每个槽的大小要么是 32 位，要么是 64 位（仅当数组存放了 `float64!` 类型的元素）。

例子：

```red
list: [123 456 789]
probe list/0                           ;-- outputs 3
probe list/2                           ;-- outputs 456

s: [#"h" #"e" #"l" #"l" #"o"]
sz: as int-ptr! s
probe sz/0                             ;-- outputs 5
probe s/5                              ;-- outputs o

a: ["hello" "Red" "world"]
probe a/0                              ;-- outputs 3
probe as-c-string a/2                  ;-- outputs Red

c: [456 "hello" "Red" "world" #"e" true]
probe c/0                              ;-- outputs 6
probe c/1
probe as-c-string c/3                  ;-- outputs Red
probe as-byte c/5                      ;-- outputs e
probe as-logic c/6                     ;-- outputs true

e: [1.23e10 "Hello" 789 "World!" 3.14]
pf: as float-ptr! e
probe pf/1                             ;-- outputs 1.23e+010
probe as-c-string e/3                  ;-- outputs Hello
probe e/5                              ;-- outputs 789
probe as-c-string e/7                  ;-- outputs World!
probe pf/5                             ;-- outputs 3.14
```

注意：

- 第二个例子中，数组字面量不等同于 `c-string!` 的 `"hello"`，因为数组字面量没有在末尾加上 NUL 值。
- 注意最后一个例子中不同下标的使用，这依据所指向的值的大小（32 位或 64 位）而定。

> **数组的写访问（Write access to arrays）**
>
> 当前数组字面量允许写访问，但没有进行边界检查，因为计划以后提供一个 `array!` 类型作为一级公民。

#### 4.8.7 Null 值（Null value）

特殊的 `null` 值是专门给 `pointer!` 指针和其他类似指针（传引用）的类型（`struct!`、`c-string!`），以及伪类型（pseudo-type）`function!` 使用的。`null` 没有指定的类型，但可以用来代替其他类似指针的值。因此，`null` 不能用来作为全局变量和本地变量的初始化值，因为 `null` 没有显式指定的类型。

`null` 是一等公民，可以赋值给变量，给函数传参，或从函数返回。

> 注意：不能显式把 `null` 转成给定的类型，只能由编译器自动进行隐式转换。

例如：

```red
a: declare pointer! [integer!]
a: null                                ;-- valid assignment, 'a type is defined
b: null                                ;-- invalid assignment, type of b cannot
                                       ;--  be deduced by the compiler

foo: func [s [c-string!] return: [c-string!]][
   if s = null [
       print "error"
       return null
   ]
   return uppercase s
]

b: foo "test"                          ;-- will set b to "TEST"
b: foo null                            ;-- will print "error" and set b to null
```

#### 4.8.8 C 语言的空指针（C void pointer）

Red/System 中没有特别支持 C 中的空指针，官方建议用 `pointer! [byte!]` 类型来表示 C 中的 `void *` 指针。

对于指向 `c-string!` 和 `struct!` 变量的指针，可以解引用后类型转换成其他目标类型。
For pointers to c-string! or struct! variables, a pointer variable can be used then dereferenced and converted using type casting to the target type.

例如：

```red
p-buffer!: alias struct! [buffer [c-string!]]

set-hello: function [
   s [p-buffer!]
][
   s/buffer: "hello"
   s                                   ;-- equivalent to C's char **
]

foo: func [
   /local
       c [p-buffer!]
][
   c: declare p-buffer!
   set-hello c
   print c/buffer
]

foo                                    ;-- call foo function
```

会打印：

```red
hello
```

> **运行时宏（Runtime macro byte-ptr!）**
>
> `Red/System` 的运行时定义了 `byte-ptr!` 宏（即定义了 `pointer! [byte!]`）作为等同于 C 语言的 `void*` 指针，用于访问原始内存（raw memory accesses）。

#### 4.8.9 变量指针（Variable pointer）

It is possible to get a pointer on an existing variable for the following datatypes:
可以获取下列类型已经存在的变量的指针：

- integer!
- byte!
- float!
- float32!

语法：

```red
:<variable>

<variable> : a variable name of allowed type.
```

这个表达式会返回变量所属类型的指针：

| variable | :variable           |
| -------- | ------------------- |
| integer! | pointer! [integer!] |
| byte!    | pointer! [byte!]    |
| float!   | pointer! [float!]   |
| float32! | pointer! [float32!] |

例子：

```red
s: declare pointer! [integer!]
a: 123
s: :a
print s/value       ;-- will output 123
```

> **本地变量指针（Pointer on local variable）**
>
> 可以获取本地变量的指针，然而，要特别注意避免一旦函数已经返回（即函数已执行结束并返回），就不能再使用其本地变量指针，这会由于栈错误导致崩溃！



### 4.9 类型转换（Type Casting）

类型转换要用 `as` 关键字跟上目标类型和待转的值。

类型转换可以在两个兼容的类型中进行，兼容的类型在下面的类型转换矩阵中定义了。运行时的类型转换可能会生成一些类型组合体。A run-time type conversion might be generated for some types combinations.

语法：

```red
as <new-type> <expr>
as [<new-type>] <expr>                 ;-- alternative syntax
as <new-type> keep <expr>

<expr>     : any valid R/S expression.
<new-type> : integer! | byte! |  logic! | c-string! | float! | float32! |
             pointer! [integer!] | struct! [<members>] |
             <alias-name>
```

注意：

- 可选的 `keep` 关键词可以确保所有位都被保留，只有类型被改变。
- 不允许多层嵌套的类型进行转换，会引发编译错误。
- 没有正确转换类型就试图把一个值赋值给不同类型的变量，会导致编译错误。

例如：

```red
foo: 0                                 ;-- foo is an integer variable
bar: declare pointer! [integer!]       ;-- bar is a pointer variable

foo: as integer! bar                   ;-- type casting
bar: as pointer! [integer!] foo
```

**类型转换矩阵（Type casting reference matrix）**

注意 `pointer!`、`c-string!`、`struct!` 和 `function!` 类型是传引用的，所以下面对于这些类型的转化是应用在它们的内存地址。

| source>>  | byte!                    | integer!          | logic!                       | c-string!            | pointer!             | struct!              | float!        | float32!    | function!    |
| --------- | ------------------------ | ----------------- | ---------------------------- | -------------------- | -------------------- | -------------------- | ------------- | ----------- | ------------ |
| byte!     | WARNING                  | as byte! ¹        | true»#"^(01)" false»#"^(00)" | ERROR                | ERROR                | ERROR                | ERROR         | ERROR       | ERROR        |
| integer!  | as integer!              | WARNING           | true»1 false»0               | as integer!          | as integer!          | as integer!          | to integer!   | to integer! | as integer!  |
| logic!    | #"^(00)"»false else»true | 0»false else»true | WARNING                      | null»false else»true | null»false else»true | null»false else»true | ERROR         | ERROR       | ERROR        |
| c-string! | ERROR                    | as c-string!      | ERROR                        | WARNING              | as c-string!         | as c-string!         | ERROR         | ERROR       | ERROR        |
| pointer!  | ERROR                    | as pointer!       | ERROR                        | as pointer!          | WARNING              | as pointer!          | ERROR         | ERROR       | as pointer!  |
| struct!   | ERROR                    | as struct!        | ERROR                        | as struct!           | as struct!           | WARNING              | ERROR         | ERROR       | ERROR        |
| float!    | ERROR                    | as float!         | ERROR                        | ERROR                | ERROR                | ERROR                | WARNING       | as float! ² | ERROR        |
| float32!  | ERROR                    | as float32!       | ERROR                        | ERROR                | ERROR                | ERROR                | as float32! ² | WARNING     | ERROR        |
| function! | ERROR                    | as function!      | ERROR                        | as function!         | as function!         | as function!         | ERROR         | ERROR       | as function! |


¹ 可以转化，但是大于 255 的整型会被截断，请注意！
² 可能会发生数据被修改。



### 4.10 Size?

语法：

```red
size? <type>
size? "<string>"

<type>     : an valid type name.
"<string>" : a c-string literal value.
```

`Size?` 以 字节为单位返回指定类型的值所占用的内存大小。而作用在 `c-string!` 字面量是，它会返回包括结尾 `null` 字节的总字节数。

例如：

```red
size? byte!                ;-- will return 1
size? integer!             ;-- will return 4
s!: alias struct! [
   a [integer!]
   b [float32!]
]
size? s!                   ;-- will return 8
```

> **动态 `string` 的案例（Dynamic string use cases）**
>
> `Size?` 只能用于 `c-string!` 字面量，而 `c-string!` 的指针只能用 `length?` 函数。

## 5. 表达式

表达式是`Red/System`程序的基本构件。包括：

- 变量
- 字面量
- 函数调用
- 运算符操作
- 圆括号中的子表达式

### 5.1 正式语法规则

以下是由[BNF](http://en.wikipedia.org/wiki/Backus%E2%80%93Naur_Form)格式指定的语法规则，定义在本地语言中的部分使用`...`表示。

```red
<literal>       ::= ... any valid Red/System literal value ...
<variable>      ::= ... any valid Red/System variable name ...
<logic-call>    ::= ... function call that returns a value of logic! type ...
<func-call>     ::= ... function call that returns a value ...
<statement>     ::= ... statement or function without return value...

<logic-literal> ::= "true" | "false"

<math-op>       ::= "+" | "-" | "*" | "/" | "//" | "%"
<bitwise-op>    ::= "and" | "or" | "xor"
<comparison-op> ::= "=" | "<>" | "<" | "<=" | ">=" | ">"
<op>            ::= <math-op> | <bitwise-op>

<cond-expr>     ::= <value> <comparison-op> <expression>
<condition>     ::= <logic-literal> | <logic-call> | <cond-expr>

<all>           ::= "ALL" "[" <condition>+ "]"
<any>           ::= "ANY" "[" <condition>+ "]" 
<either>        ::= "EITHER" <condition> "[" <expression>+ "]" "[" <expression>+ "]"
<short-circuit> ::= <all> | <any> | <either>

<paren>         ::= "(" <expression> ")"
<value>         ::= <variable> | <literal> | <paren> | "null"

<infix>         ::= <value> <op> <expression>
<prefix>        ::= <func-call> <expression>* | <statement> <expression>* 
<call>          ::= <prefix>+ | <infix> | <cond-expr>

<expression>    ::= <call> | <value> | <short-circuit>
```

表达式可以单独使用，跟在赋值语句后或者作为一些语句的参数（`RETURN`，`IF`或者`EITHER`都被用作语句）。

注意：`EITHER`在被用于表达式中时，其`TRUE`块和`FALSE`块中最后一个表达式的值必须为相同类型。类似于C语言中的三元运算符（?）。

**示例**

```red
a: 123
foo a + 1
0 < foo a + 1
any [(0 < foo a + 1) a > 0]

if any [(0 < foo a + 1) a > 0][print "ok"]

b: 1 + (2 * a - either zero? a [0][a + 100])
```

### 5.2 求值顺序规则

表达式从左到右求值，除了中缀运算符优先级高于前缀运算符（包括函数调用）的规则之外，没有其他运算符优先级。

**示例**

```red
1 + 2 * 3                              ;-- (1 + 2) * 3 returns 9
1 + 2 * 3 = 9                          ;-- ((1 + 2) * 3) = 9 returns TRUE
9 = 1 + 2 * 3                          ;-- ((9 = 1) + 2) * 3 raises an error!

1 + (2 * 3)                            ;-- 1 + (2 * 3) returns 7

foo 1 + 2                              ;-- foo (1 + 2)
1 + foo 2 * 3                          ;-- 1 + (foo (2 * 3))
```
