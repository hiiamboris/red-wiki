* Red 层调用样例
```
#import %lib-mysql.red ; red语言的mysql库头文件

MYSQL: mysql_init 0 
mysql_real_connect MYSQL "主机名" "用户名"  "密码" "库名"   端口  unix-socket路径  flag

```

* Red层库样例
```
lib-handle: 0
mysql_init: func[ MYSQL [integer!] return: [integer!] /local  ret [integer!] ][
   lib-handle: libopen "libmysqlclient.so" 
   ret: libcall lib-handle "mysql_init"  MYSQL 
   ret      
]
```


* Red/System层

* Red运行环境

* 例子