---
title: MySQL基础知识
key: 2019-10-15
tags: MySQL
---

### 1. win10以管理员权限打开cmd
右键开始菜单，选择命令提示符（管理员）
### 2. 常用命令
开启mysql服务  
`net start mysql`  
关闭mysql服务  
`net stop mysql`  
登陆mysql  
`mysql -u root -p` 我的密码是123456  
### 3. 常见问题
##### 1. Navicat连接不上
错误内容： 1251-Client does not support authentication protocol requested by server  
错误原因： mysql的插件处理密码的格式和长度不一样导致的  
解决方法：  
```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
FLUSH PRIVILEGES;
```

----

[navicat for mysql 链接时报错](https://my.oschina.net/u/3295928/blog/1811804)  
[navicat for mysql 10.0.11简体中文破解版 绿色版](https://blog.csdn.net/qq_16093323/article/details/79228481)
