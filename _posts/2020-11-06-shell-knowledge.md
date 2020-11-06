---
title: shell基础知识
key: 2020-11-06
tags: shell
---

### 1. -n来判定字符串非空
```sh
ARGS=$*
if [ -n "$ARGS" ] then
   print "with argument"
fi
print "without argument"
```
变量必须加双引号
### 2. 参数的含义
- $#：传入脚本的参数个数；
- $0: 脚本自身的名称；　　
- $1: 传入脚本的第一个参数；
- $2: 传入脚本的第二个参数；
- $@: 传入脚本的所有参数；
- $*：传入脚本的所有参数；
- $$: 脚本执行的进程id；
- $?: 上一条命令执行后的状态，结果为0表示执行正常，结果为1表示执行异常；

其中$@与$*正常情况下一样，当在脚本中将$*加上双引号作为“$*”引用时，此时将输入的所有参数当做一个整体字符串对待。比如输入参数有a b c三个参数，则“$*”表示“a b c”一个字符串。

----

[Linux shell if [ -n ] 正确使用方法](https://www.cnblogs.com/ariclee/p/6137456.html)  
[Linux脚本中$#、$0、$1、$@、$*、$$、$?](https://www.cnblogs.com/quxiangxiangtiange/p/10290715.html)   

