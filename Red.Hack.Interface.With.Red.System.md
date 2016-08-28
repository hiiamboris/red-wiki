Red语言和Red/System之间的交互

* Red语言提供了更多的类型，Red/System只有有限的几种数据类型
* Red/System提供了指针类型，这是Red所没有的
* Red和Red/System交互时，只有logic! integer! 可以直接传递
* 用来交互的类型在  runtime/datatypes/structures.reds 中进行了定义
* 原则：在R/S中使用 red-XXXX 类型来和  Red 中的数据类型相对应
* 问题：不定参数也可以么？