---
title: Linux(Centos) python开发环境配置
key: 2020-03-21
tags: Linux
---

### 1. 查看ip
`ip addr`     
`ifconfig`  
`如果此命令不能用   
`yum install net-tools`
### 2. 安装命令
Ubuntu用apt，centos用yum   
### 3. 无法联网
测试是否联网`ping www.baidu.com`   
`vi /etc/resolv.conf` (添加 nameserver 114.114.114.114)   
若还是无法联网
`vi /etc/sysconfig/network-scripts/ifcfg-xx` 将改ONBOOT=yes， 再重启网络服务   
`service network restart`
### 4. 命令行终端远程连接虚拟机
将虚拟机的网络设置改为桥接，选择wifi网卡   
`ssh root@192.108.0.108`   
命令行终端可以用xshell，MobaXterm，putty等
### 5. 替换默认源
http://mirrors.163.com/.help/centos.html   
1.备份/etc/yum.repos.d/CentOS-Base.repo  
`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`   
2.下载对应版本repo文件, 放入/etc/yum.repos.d/   
`cd /etc/yum.repos.d`   
`wget http://mirrors.163.com/.help/CentOS7-Base-163.repo`若没有wget命令
`curl http://mirrors.163.com/.help/CentOS7-Base-163.repo -o CentOS7-Base-163.repo`   
`yum clean all`   
`yum makecache`  
安装所需要的软件   
`yum install wget` 
`yum install vim`  
`yum install gcc`  
`yum install kernel-devel`  
`yum install kernel-headers`
### 6. 安装mysql数据库
1.CentOS7默认安装mariadb数据库，将其删除   
`yum remove mariadb-libs.x86_64`   
2.下载Mysql源  
`wget https://repo.mysql.com/mysql57-community-release-el7-8.noarch.rpm`  
3.安装源  
`yum localinstall mysql57-community-release-el7-8.noarch.rpm`  
4.安装MySQL  
`yum install mysql-community-server`  
5.启动mysql  
`sudo service mysqld start/restart/stop`  
6.默认密码  
`cat /var/log/mysqld.log | grep password`
7.登陆mysql  
`mysql -uroot -p`  
8.更改密码  
`mysql> SET PASSWORD = PASSWORD('123456');`  
现在是不允许设置简单密码的，需要设置以下两个参数  
`mysql> set global validate_password_policy=0;`  
`mysql> set global validate_password_length=1;`    
### 7. 安装python
```shell
1.	yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel  mysql-devel gcc gcc-devel python-devel  
2.	wget "https://www.python.org/ftp/python/3.5.0/Python-3.5.0.tgz"
3.	tar -zxvf Python-3.5.0.tgz
4.	mkdir /usr/local/python3
5.	Python-3.5.0/configure --prefix=/usr/local/python3
6.	make && make install
7.	ln -s /usr/local/python3/bin/python3 /usr/bin/python3

更改pip源
创建或修改文件：~/.pip/pip.conf
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

国内常见源
阿里云 http://mirrors.aliyun.com/pypi/simple/
豆瓣(douban) http://pypi.douban.com/simple/
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

安装pip3 和 virtualenv
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
pip3 install virtualenv
ln -s /usr/local/python3/bin/virtualenv /usr/bin/virtualenv
virtualenv -p /usr/bin/python3 test_env
source ~/wechat_env/bin/activate  启动虚拟工作空间
```

----

[使用VirtualBox设置共享文件夹](https://blog.csdn.net/MingL_Wang/article/details/45156019)  
