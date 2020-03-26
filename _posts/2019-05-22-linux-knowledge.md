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
`tar -zxvf file.tar.gz -C dirName` 解压tar包到指定目录  
`tar -zcvf file.tar.gz dirName`    指定目录压缩到tar包  
`unzip file.zip -d dirName` 解压zip包到指定目录  
`zip -r file.zip dirName`   指定目录压缩到zip包
### 5. tail
`tail -f file` 实时输出文件更新的内容
### 6. cp
cp 在linux下是复制的命令  
-r 就会将源目录下的所有文件夹以及文件下的文件复制到目标目录下  
-i 会在复制文件的时候给提示,如果复制的目标文件存在,会给你提示,不存在就直接复制了  
-f 强行复制文件或目录，不论目标文件或目录是否已存在  
\cp 取消cp的别名
### 7. scp
scp 可以在 2个 linux 主机间复制文件  
命令基本格式： `scp [可选参数] file_source file_target`  
`scp local_file remote_username@remote_ip:remote_folder`    
`scp local_file remote_username@remote_ip:remote_file`  
`scp local_file remote_ip:remote_folder`  
`scp local_file remote_ip:remote_file`  

第1,2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名
第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名
例子:  
`scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music`  
`scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music/001.mp3`   
`scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music`  
`scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music/001.mp3`   
### 8. yes
在删除n个文件时，需要确认要不要删除  
`yes | rm -i *`  删除时，自动回答y，就删除了
### 9. alias
`alias` 查看系统中所有的命令别名  
`alias 别名='原命令'` 设置别名  
`unalias 别名` 删除别名  
`~/.bashrc` 写入这个文件中使别名永久生效
### 10. 查看当前进程
`ps -ef | grep username` 查看当前进程  
`kill -9 进程号` 关闭此进程
### 11. find & grep
`find . -name filename` 查找文件名称  
`grep "content" . -r -n` 查找关键内容  
`--exclude=GLOB` 排除指定类型的文件  
`--include=GLOB` 从指定类型的文件中搜索
### 12. PS1命令提示符
`export PS1="\[\e]2;\w\a\e[31;1m\]\u\[\e[30;1m\]@\[\e[34;1m\][\H \W]\\$ \[\e[0m\]"`  
kefuz@[xhdrdevl27 ~]$
### 13. 更改分辨率
`xrandr -s 1920x1080`
### 14. shell执行多个命令
（1）在每个命令之间用；（分号）隔开。  
（2）在每个命令之间用&&隔开。  
 &&表示：若前一个命令执行成功，才会执行下一个。这样，可确保所有的命令执行完毕后，其执行过程都是成功的
这种执行方法，经常运用在自动安装的Script中。  
（3）在每个命令之间用||隔开。  
 ||表示：若前一个命令执行成功，就不会执行下一条了。  
 
----

[在VirtualBox下安装linux操作系统](https://blog.csdn.net/yuchao2015/article/details/52132270)  
[Unix环境下PS1变量的设置](https://www.cnblogs.com/undead/p/3741764.html)  
[linux shell 执行多个命令的方法](https://blog.csdn.net/ysdaniel/article/details/6127860)  
