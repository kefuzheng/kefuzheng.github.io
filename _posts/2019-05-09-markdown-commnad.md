---
title: Markdown基础语法
key: 2019-05-09
tags: Markdown
---

### 1. 标题
在想要设置为标题的文字前面加#来表示  
一个#是一级标题，二个#是二级标题，以此类推。支持六级标题。  
注：标准语法一般在#后跟个空格再写文字，貌似简书不加空格也行。
	
### 2. 分割线
三个或者三个以上的 - 或者*。 
	
### 3. 图片
语法：  
`![图片alt](图片地址 ''图片title'')`  
图片alt就是显示在图片下面的文字，相当于对图片内容的解释。  
图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加  
先将图片上传github，然后选中图片，点击download，在地址栏中复制地址即可
![test](https://raw.githubusercontent.com/kefuzheng/kefuzheng.github.io/master/assets/images/open-graph-template.png)

### 4. 表格
语法：  
表头|表头|表头  
---|:--:|---:  
内容|内容|内容  
内容|内容|内容  
第二行分割表头和内容。  
- 有一个就行，为了对齐，多加了几个  
文字默认居左  
-两边加：表示文字居中  
-右边加：表示文字居右  
注：原生的语法两边都要用 | 包起来。此处省略  
	
### 5. 换行
空格+空格+回车  
       
### 6. 代码
单行代码：代码之间分别用一个反引号包起来  
代码块：代码之间分别用三个反引号包起来，且两边的反引号单独占一行
```java
public static void main(String[] args){
    int a = 0;
    int b = 1;
    int c = a + b;
}
```

----

[Markdown基础语法](https://www.jianshu.com/p/191d1e21f7ed)
