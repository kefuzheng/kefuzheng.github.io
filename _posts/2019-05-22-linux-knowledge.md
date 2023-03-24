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
`zip -d xxx.zip "WEB-INF/web.xml"` 删除xxx.zip压缩包里WEB-INF目录下的web.xml文件
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
（1）在每个命令之间用;（分号）隔开。  
`command1;command2` 分号连接的命令会按照顺序从前向后依次执行，但分号两端的命令之间没有任何逻辑关系，所有写出来的命令最终都会被执行，即使分号前面的命令出错也不影响后面的命令
（2）在每个命令之间用&&隔开。  
`command1  &&  command2`   
 &&表示：逻辑与。&&连接的命令会按照顺序从前向后执行，但只有当command1正确执行才执行command2，如果command1不正确执行，则不执行command2。  
这种执行方法，经常运用在自动安装的Script中。  
（3）在每个命令之间用||隔开。  
`command1 || command2`   
 ||表示：逻辑或。||连接的命令会按照顺序从前向后执行，但只有当command1不正确执行才执行command2，command1正确执行则不会执行command2  
 (4)&
 `command1 &`   
 `command1 & command2`   
 &表示将其前面的命令放入后台执行，放入后台后会立即返回到bash环境让用户可以继续和bash交互。如果&符号连接了两个命令，则其前面的命令被放入后台，立即执行后面的命令，所以可以简单地认为这两个命令是并行执行的，两端的命令之间也没有任何逻辑关系。
 (5)|
 `command1 | command2`   
 |叫做管道符，可以将command1的结果作为command2的输入
### 15. bsub
##### 1. 命令格式：
  `bsub [options] command [argument]`
##### 2. 参数含义 
```
-q   选择队列
-i   指定输入文件  
-I   交互模式，此时终端不能输入
-o   指定输出文件，作业提交后标准输出的信息会保存到这个文件中。
-e   指定输出文件，作业提交后标准错误输出的信息会保存到这个文件中。
-n   指定作业需要的CPU核
-J   作业的名字
```
##### 3. 命令
```
bjobs：查看系统的job
bsub：提交作业
bhist：查看作业历史
bkill：kill一个作业
```
### 16. Linux下批量Kill多个进程
`ps -ef|grep php|grep -v grep|cut -c 9-15|xargs kill -9`
- "ps - ef"是linux 里查看所有进程的命令。这时检索出的进程将作为下一条命令"grep php"的输入。  
- "grep php"的输出结果是，所有含有关键字"php"的进程。  
- "grep -v grep"是在列出的进程中去除含有关键字"grep"的进程。
- "cut -c 9-15"是截取输入行的第9个字符到第15个字符，而这正好是进程号PID。
- "xargs kill -9"中的xargs命令是用来把前面命令的输出结果（PID）作为"kill -9"命令的参数，并执行该令。

### 17. 查看桌面环境和Gtk
1. echo $DESKTOP_SESSION
2. ps -A | egrep -i "gnome|kde|mate|cinnamon|lxde|xfce|jwm"
3. dpkg -l libgtk2.0-0 libgtk-3-0

### 18. sudo
`sudo -s` 进入root
### 19. Ubuntu卸载软件
```shell
apt-get --purge remove <package>		# 删除软件及其配置文件
apt-get autoremove <package>				# 删除没用的依赖包
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P	# 清理dpkg的列表中有“rc”状态的软件包
```
### 20. 不能修改文件权限
`sudo chattr -i libprotobuf.so` # 注意chattr修改文件属性需要root权限   
`chmod 544 libprotobuf.so`

### 21. ubuntu中查找软件的安装位置
ubuntu中可供调用的终端大都在/usr/bin或者/opt，但也不尽然。可尝试用下面的方法快速找到软件的位置。
1. 执行该程序；
2. 用命令 ps -e 找到该程序的名字；
3. 用 find 或 whereis 命令查找文件位置

使用apt-get install命令安装的软件，可直接用命令 dpkg -S softwarename 显示包含此软件包的所有位置，dpkg -L softwarename 显示安装路径。
### 22. env设置
Linux的变量种类：按变量的生存周期来划分，Linux变量可分为两类：
1. 永久的：需要修改配置文件，变量永久生效。
2. 临时的：使用export命令声明即可，变量在关闭shell时失效。

##### 1. 设置变量的三种方法
1. 在/etc/profile文件中添加变量【对所有用户生效(永久的)】，修改文件后要想马上生效还要运行# source /etc/profile不然只能在下次重进此用户时生效。
2. 在用户目录下的.bash_profile文件中增加变量【对单一用户生效(永久的)】，修改文件后要想马上生效还要运行$ source /home/guok/.bash_profile不然只能在下次重进此用户时生效。
3. 直接运行export命令定义变量【只对当前shell(BASH)有效(临时的)】

### 23. 查看文件被那个进程占用
##### 1. fuser
`fuser -v -u file`   
fuser可以用来查看文件或目录被谁占用。-v：详细显示；-u：显示用户；-m：显示陌路下所有文件和目录被占用情况；-k：杀掉占用文件的进程，配合-i可以让用户选择是否删除   
##### 2. lsof
`lsof  /tmp/nginx_access.log`  查看文件被那些进程打开   
`lsof  +d /tmp/`  查看目录先被占用的文件及进程   
查看指定用户占用的文件，使用选项-u，后面跟用户名

### 24. 在linux下怎么安装.bin文件
$ cd 你希望安装的目录
$ chmod a+x  j2sdk-1_4_2-nb-3_5_1-bin-linux.bin
$ ./j2sdk-1_4_2-nb-3_5_1-bin-linux.bin
然后就会出现协议，问你同意否，yes继续安装......

### 25. 查看终端是csh还是bash和更改shell设置
`echo $SHELL` 查看   
`chsh -s /bin/bash` 更改

### 26. Docker
- docker images //查看当前本地存在的镜像
- docker search name //搜索镜像
- docker pull name //拉取镜像，name是镜像名
- docker run -d IMAGE //执行镜像， -d是后台执行
- docker ps //查看在docker上运行的程序
- docker stop IMAGE //停止后台启动的镜像
- docker exec -it CONTAINER_ID bash //进入docker启动的镜像中， -i在没有attach的时候保证用户的input有效，-t分配一个伪终端，bash使用shell
- docker restart CONTAINER_ID //重启容器

##### 1. Nginx
`docker run -d -p 8080:80 hub.c.163.com/library/nginx` //docker 开放端口给外部网络访问, -p（小） 是开放指定的端口，与上面，开本机端口8080映射到nginx的80端口    
`docker run -d -P hub.c.163.com/library/nginx`  // -P(大） 开放所有的端口与主机建立映射
`docker ps`  // 可以查看进程，并看端口，连接   
`netstat -na | grep portID` // 查看网络状态  

##### 2. 制作自己的镜像
1. 下载tomcat镜像
`docker pull hub.c.163.com/library/tomcat:latest`  
2. 编写自己的镜像
```shell
vim Dockerfile
from hub.c.163.com/library/tomcat
MAINTAINER Joon xx@163.com
COPY jpress.war /usr/local/tomcat/webapps
```
3. 构建镜像
`docker build -t jpress:latest .`
4. 查看镜像
`docker images`

### 27. 使用ssh工具连接到WSL
```bash
# 卸载重装一遍ssh服务
sudo apt-get remove openssh-server
sudo apt-get install openssh-server

# 编辑sshd_config文件，修改几处配置才能正常使用用户名/密码的方式连接
Port 22 #默认即可，如果有端口占用可以自己修改
PasswordAuthentication yes # 允许用户名密码方式登录

# 修改完之后重启ssh服务
sudo service ssh restart
```

### 28. 查看文件被哪个进程占用
`fuser -uv .` 查看当前目录正在被哪些进程占用   
`fuser -uv ./test.log` 查看文件正在被哪些进程占用   


----

[在VirtualBox下安装linux操作系统](https://blog.csdn.net/yuchao2015/article/details/52132270)  
[Unix环境下PS1变量的设置](https://www.cnblogs.com/undead/p/3741764.html)  
[linux shell 执行多个命令的方法](https://blog.csdn.net/ysdaniel/article/details/6127860)  
[LSF 作业管理系统](https://www.jianshu.com/p/601ca9f33b31)   

