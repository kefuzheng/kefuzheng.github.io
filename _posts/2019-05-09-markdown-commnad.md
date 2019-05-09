---
title: Markdown基础语法
key: 2019-05-09
tags: Markdown
---

### 1. 标题  
	在想要设置为标题的文字前面加#来表示  
	一个#是一级标题，二个#是二级标题，以此类推。支持六级标题。  
	注：标准语法一般在#后跟个空格再写文字，貌似简书不加空格也行。  
	示例：
	(```)
	# 这是一级标题
	## 这是二级标题
	### 这是三级标题
	#### 这是四级标题
	##### 这是五级标题
	###### 这是六级标题
	(```)
### 2. 分割线
	三个或者三个以上的 - 或者*。 
### 3. 图片
	语法：  
	`![图片alt](图片地址 ''图片title'')`  
	图片alt就是显示在图片下面的文字，相当于对图片内容的解释。  
	图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加  

	示例： 
        (```)
	![blockchain](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/
	u=702257389,1274025419&fm=27&gp=0.jpg "区块链")
	(```)
### 4. 表格
	语法： 
        (```)	
	表头|表头|表头  
	---|:--:|---:  
	内容|内容|内容  
	内容|内容|内容  
        (```)
	第二行分割表头和内容。  
	- 有一个就行，为了对齐，多加了几个  
	文字默认居左  
	-两边加：表示文字居中  
	-右边加：表示文字居右  
	注：原生的语法两边都要用 | 包起来。此处省略  

	示例：
	(```)
	姓名|技能|排行  
	--|:--:|--:  
	刘备|哭|大哥  
	关羽|打|二哥  
	张飞|骂|三弟  
        (```)

### 5. 换行
       空格+空格+回车

[Markdown基础语法](https://www.jianshu.com/p/191d1e21f7ed)