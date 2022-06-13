---
title: Windows基础知识
key: 2021-09-17
tags: Windows
---

### 1. Powershell下设置环境变量
`ls env:`#查看所有环境变量    
`ls env:NODE*`#搜索环境变量   
`$env:NODE_ENV`#查看单个环境变量   
`$env:NODE_ENV=development`#添加/更新环境变量    
`del evn:NODE_ENV`#删除环境变量

### 2. cmd设置环境变量
`set`#查看所有环境变量   
`set NODE_ENV`#查看单个环境变量   
`set NODE_ENV=development`#添加/更新环境变量    
`set NODE_ENV=`#删除环境变量

### 3. 结束进程
1. 进入cmd
2. tasklist 显示进程
3. 结束进程
`taskkill /f /t /im qq.exe`   
`taskkill /pid [进程码] -t -f`: -t结束该进程, -f强制结束该进程以及所有子进程, `taskkill /pid 10580 -f `   
`tskill 6528`

### 4. 查看指定端口的使用情况
`netstat -ano | findstr 端口号`

----


