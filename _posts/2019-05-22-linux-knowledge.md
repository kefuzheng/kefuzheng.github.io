---
title: Linux基础知识
key: 2019-05-22
tags: Linux
---

### 1. 设置root密码
`sudo passwd`设置root密码  
`su root`切换root用户
### 2. link
`ln -s destfile/directory softlink` 建立软连接(类似于Windows的快捷方式)   
`ln destfile hardlink`              建立硬连接
### 3. df & du
`df -h ./`        查看使用情况
`du -sh filename` 查看文件的大小
### 4. 解压
`tar -zxvf file.tar -C dirName` 解压tar包到指定目录  
`unzip file.zip -d dirName`     解压zip包到指定目录
### 5. tail
`tail -f file` 实时输出文件更新的内容
### 6. cp
cp 在linux下是复制的命令  
-r 就会将源目录下的所有文件夹以及文件下的文件复制到目标目录下  
-i 会在复制文件的时候给提示,如果复制的目标文件存在,会给你提示,不存在就直接复制了  
-f 强行复制文件或目录，不论目标文件或目录是否已存在  
\cp 取消cp的别名
### 7. yes
在删除n个文件时，需要确认要不要删除  
`yes | rm -i *`  删除时，自动回答y，就删除了

----

[在VirtualBox下安装linux操作系统](https://blog.csdn.net/yuchao2015/article/details/52132270)  
