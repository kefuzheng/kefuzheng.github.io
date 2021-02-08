---
title: Makefile基础知识
key: 2021-02-08
tags: Makefile
---

### 1. wildcard用法
$(wildcard PATTERN...) 。在Makefile中，它被展开为已经存在的、使用空格分开的、匹配此模式的所有文件列表
```makefile
src=$(wildcard *.c ./sub/*.c)
dir=$(notdir $(src))
obj=$(patsubst %.c,%.o,$(dir) )

all:
    @echo $(src)
    @echo $(dir)
    @echo $(obj)
    @echo "end"

# 第一行输出：
# a.c b.c ./sub/sa.c ./sub/sb.c

# wildcard把 指定目录 ./ 和 ./sub/ 下的所有后缀是c的文件全部展开。

# 第二行输出：
# a.c b.c sa.c sb.c
# notdir把展开的文件去除掉路径信息
 
# 第三行输出：
# a.o b.o sa.o sb.o
 
# 在$(patsubst %.c,%.o,$(dir) )中，patsubst把$(dir)中的变量符合后缀是.c的全部替换成.o，
# 或者可以使用'obj=$(dir:%.c=%.o)'效果也是一样的。
```

 
----

[GDB调试C/C++程序](http://c.biancheng.net/view/8153.html)  
[GDB调试多线程程序](http://c.biancheng.net/view/8261.html)   

