---
title: Git常用命令
key: 2019-05-09
tags: Git
---

### 1. 查看连接ssh是否成功
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
	
[Git基本操作](http://www.runoob.com/git/git-basic-operations.html)  
[搭建Github博客](https://blog.csdn.net/u012168038/article/details/77715439)
