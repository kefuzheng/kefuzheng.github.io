---
title: nodejs基础知识
key: 2020-11-06
tags: nodejs
---

### 1. 取得当前执行路径
- process.cwd() 当前执行程序的路径（执行命令行时候的路径,不是代码路径 例如 在根目录下执行 node ./xxx/xxx/a.js 则 cwd 返回的是 根目录地址 ）
- __dirname: 代码存放的位置
- process.execPath: 当前执行的node路径（如：/bin/node）


----

[nodejs实现文件的拷贝复制](https://www.cnblogs.com/coding4/p/7495968.html)  
