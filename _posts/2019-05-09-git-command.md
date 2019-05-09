---
title: Git常用命令
key: 2019-05-09
tags: Git
---
1. 查看连接ssh是否成功  
   ssh -T git@github.com
2. 克隆github上的仓库到本地  
   git clone [url]
3. 提交代码常用命令  
	git add ./-all/*.java/test.java 将文件添加到缓存区  
	git status -s 查看项目的当前状态  
	git commit -m "comment" 添加注释  
	git remote add origin [url] 将文件添加到远程仓库缓存区  
		如果出现错误:  
        fatal: remote origin already exists  
        则执行以下语句：  
        git remote rm origin  
	    再执行git remote add origin [url]  
    git pull origin master  从master分支更新代码到本地  
    git push origin master  提交本地代码到master分支  
	
[搭建Github博客]https://blog.csdn.net/u012168038/article/details/77715439)

<!--more-->

---

If you like TeXt, don't forget to give me a star. :star2:

<iframe src="https://ghbtns.com/github-btn.html?user=kitian616&repo=jekyll-TeXt-theme&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>