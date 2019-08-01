---
title: Git常用命令
key: 2019-05-09
tags: Git
---

### 1. Git SSH Key 生成步骤
##### 1. 设置Git的user name和email
`git config --global user.name "kefuzheng" `  
`git config --global user.email "kefuzheng@hotmail.com" `  
##### 2. 查看密钥是否存在
`cd ~/.ssh`  
若存在，备份删除
##### 3. 生成密钥
`ssh-keygen -t rsa -C "kefuzheng@hotmail.com"`  
按3个回车，密码为空  
最后得到两个文件：id_rsa和id_rsa.pub  
tips: 所有平台以及所有私有电脑使用的都是同一个ssh公钥，当要在另一台未添加ssh的电脑上使用git，只需要使用  
`ssh-keygen -t rsa -C "kefuzheng@hotmail.com"`  
生成秘钥，然后将其他电脑的秘钥拷贝覆盖，就不需要再次在每一个git平台添加公钥.   
优点：省时省力，不用重复添加   
缺点：无法通过git平台删除指定电脑的ssh，删除公钥后所有电脑都无法接入git平台.  
##### 4. 在github上添加ssh密钥
id_rsa.pub里面的公钥
##### 5. 测试是否成功
`ssh -T git@github.com`  

### 2. 克隆github上的仓库到本地
`git clone [url]`

### 3. 提交代码常用命令
`git add ./-all/*.java/test.java` 将文件添加到缓存区  
`git status -s` 查看项目的当前状态  
`git commit -m "comment"` 添加注释  
`git remote add origin [url]` 将文件添加到远程仓库缓存区  
如果出现错误:  
fatal: remote origin already exists  
则执行以下语句：  
`git remote rm origin`  
再执行git remote add origin [url]  
`git pull origin master`  从master分支更新代码到本地  
`git push origin master`  提交本地代码到master分支  
git rm  
如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 Changes not staged for commit 的提示。  
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除，然后提交。可以用以下命令完成此项工作  
`git rm <file>`  
如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f  
可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件：  
`git rm –r *` 
git mv 命令用于移动或重命名一个文件、目录、软连接。
### 4. HTTPS和SSH方式的区别
HTTPS: 使用https url克隆对初学者来说会比较方便，复制https url然后到git Bash里面直接用clone命令克隆到本地就好了，但是每次fetch和push代码都需要输入账号和密码，这也是https方式的麻烦之处。  
SSH: 使用SSH url克隆却需要在克隆之前先配置和添加好SSH key，因此，如果你想要使用SSH url克隆的话，你必须是这个项目的拥有者。否则你是无法添加SSH key的，另外ssh默认是每次fetch和push代码都不需要输入账号和密码，如果你想要每次都输入账号密码才能进行fetch和push也可以另外进行设置
### 5. .gitignore文件
`<repository path>/.gitignore` 忽略指定类型的文件
### 6. 修改Github上的开源代码
先将别人的代码Fork到自己的账户，然后进行修改，修改完成后，发起New pull requst，若原项目主同意别人所发起的request，就会把被人修改的内容更新到原项目

----

[Git基本操作](http://www.runoob.com/git/git-basic-operations.html)  
[搭建Github博客](https://blog.csdn.net/u012168038/article/details/77715439)
