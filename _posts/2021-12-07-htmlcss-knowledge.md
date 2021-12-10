---
title: HTML+CSS基础知识
key: 2021-12-07
tags: HTML+CSS
---

### 1. 常用标签
##### 1. 标题<hx>
`<hx>标题文本</hx>`(x为1-6)

##### 2. span
`<span>文本</span>`没有语义的，它的作用就是为了设置单独的样式用的。

##### 3. div
`<div>…</div>` 相当于容器

##### 4. header
`<header>我是头部标签</header>` 定义头部区域

##### 5. footer
`<footer>我是底部标签</footer>` 定义底部区域

##### 5. section
`<section>我是一个区域</section>` 定义一个区域

##### 6. aside
`<aside>我是侧边栏</ aside>` 定义侧边栏

##### 7. 换行
`<br/>` 换行

##### 8. 空格
`&nbsp;` 空格

##### 9. 水平线
`<hr/>` 水平线

##### 10. 无序列表
```html
<ul>
  <li>信息</li>
  <li>信息</li>
   ......
</ul>
```

##### 11. 有序列表
```html
<ol>
   <li>信息</li>
   <li>信息</li>
   ......
</ol>
```

##### 12. img
`<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">`   
- src: 标识图像的位置；
- alt：指定图像的描述性文本，当图像不可见时（下载不成功时），可看到该属性指定的文本；
- title：提供在图像可见时对图像的描述(鼠标滑过图片时显示的文本)；
- 图像可以是GIF，PNG，JPEG格式的图像文件。

##### 13. 超链接
`<a  href="目标网址"  title="鼠标滑过显示的文本" target="_self|_blank">链接显示的文本</a>`   
target属性，代表打开网页的方式。可选值为”_self和_blank”，默认值为_self，代表在当前页面打开链接，_blank代表在新窗口打开链接。

##### 14. table
```html
<table border="1">
    <caption>前端三剑客</caption>
    <tr>
        <th>知识点</th>
        <th>重要程度</th>
        <th>难度</th>
        <th>学习周期</th>
    </tr>
    <tr>
        <td>html</td>
        <td>5星</td>
        <td>3星</td>
        <td>7天</td>
    </tr>
    <tr>
        <td>css</td>
        <td>5星</td>
        <td>4星</td>
        <td>10天</td>
    </tr>
</table>
```
1. <table>…</table>：整个表格以<table>标记开始、</table>标记结束。
2. <tr>…</tr>：表格的一行，所以有几对tr 表格就有几行。
3. <td>…</td>：表格的一个单元格，一行中包含几对<td>...</td>，说明一行中就有几列。
4. <th>…</th>：表格的头部的一个单元格，表格表头。
5. 表格中列的个数，取决于一行中数据单元格的个数。
6. border属性可以为表格添加边框，属性值为数字; table表格在没有添加border属性之前, 在浏览器中显示是没有表格线的
7. caption标签用来定义表格的标题。

```html
<table border="1">
    <thead>
        <tr>
            <th>科目</th>
            <th>分数</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>语文</td>
            <td>99</td>
        </tr>
        <tr>
            <td>数学</td>
            <td>60</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>总分</td>
            <td>159</td>
        </tr>
    </tfoot>
</table>
```
如果不加<thead><tbody><tfooter> , table表格加载完后才显示。加上这些表格结构， tbody包含行的内容下载完优先显示，不必等待表格结束后在显示，同时如果表格很长，用tbody分段，可以一部分一部分地显示。（通俗理解table 可以按结构一块块的显示，不在等整个表格加载完后显示。）   
thead、tfoot 以及 tbody 元素使您有能力对表格中的行进行分组。当您创建某个表格时，您也许希望拥有一个标题行，一些带有数据的行，以及位于底部的一个总计行。这种划分使浏览器有能力支持独立于表格标题和页脚的表格正文滚动。当长的表格被打印时，表格的表头和页脚可被打印在包含表格数据的每张页面上。

##### 15. form表单
```html
<form   method="传送方式"   action="服务器文件">

<form>
  姓名：
  <input type="text" name="myName" placeholder="请输入用户名">
  <br/>
  密码：
  <input type="password" name="pass">
</form>
```
- type：type="text"，输入框为文本输入框; type="password", 输入框为密码输入框。
- name：为文本框命名，以备后台程序ASP 、PHP使用。
- value：为文本输入框设置默认值。(一般起到提示作用）
- placeholder：为输入框占位符,里面可以放提示的输入信息。

##### 16. input
`<input type="number">` 数字输入框    
`<input type="url">` 网址输入框，需以http://或者https://开头,且后面必须有内容,否则表单提交的时候会报错误提示。    
`<input type="email">` 邮箱输入框，必须包含@，@之后必须有内容,否则会报错误提示。   
`<input type="submit" value="提交">` 提交按钮    
`<input type="reset" value="重置">` 重置按钮，重置表单信息   
`<input type="radio/checkbox" value="值" name="名称" checked="checked"/>`   
- type: type="radio"，控件为单选框; type="checkbox"，控件为复选框
- value：提交数据到服务器的值（后台程序PHP使用）
- name：为控件命名，以备后台程序 ASP、PHP 使用
- checked：当设置 checked="checked" 时，该选项被默认选中
- 同一组的单选按钮，name 取值一定要一致，这样同一组的单选按钮才可以起到单选的作用。

##### 17. select下拉菜单
```html
<select>
    <option value="看书">看书</option>
    <option value="旅游"  selected="selected">旅游</option>
    <option value="运动">运动</option>
    <option value="购物">购物</option>
</select>
```
- option标签放选项内容，不放置其他标签。
- value是向服务器提交的值
- selected="selected"，则该选项就被默认选中。


##### 17. textarea
`<textarea  rows="行数" cols="列数">文本</textarea>` 文本域   
可用css样式的width和height来代替：col用width、row用height来代替。

##### 18. label
`<label for="控件id名称">` 当用户单击选中该label标签时，浏览器就会自动将焦点转到和标签相关的表单控件上（就自动选中和该label标签相关连的表单控件上）。   
标签的 for 属性中的值应当与相关控件的 id 属性值一定要相同。

### 2. CSS
```css
p{
   font-size:12px;
   color:red;
}
```
声明组成，而声明又由属性和值组成。    
选择符：又称选择器，指明网页中要应用样式规则的元素   
声明：在英文大括号“｛｝”中的的就是声明，属性和值之间用英文冒号“：”分隔。当有多条声明时，中间可以英文分号“;”分隔   
优先级: 内联式 > 嵌入式 > 外部式, 嵌入式css样式的位置一定在外部式的后面。

##### 1. 内联式css样式
`<p style="color:red;font-size:12px">这里文字是红色。</p>` css样式代码要写在style=""双引号中，如果有多条css样式代码设置可以写在一起，中间用分号隔开

##### 2. 嵌入式css样式
```css
<style type="text/css">
span{
  color:red;
}
</style>
```

##### 3. 外部式css样式
`<link href="base.css" rel="stylesheet" type="text/css" />` 

##### 4. 设置字体
- 字体系列：`font-family:"Microsoft Yahei";`
- 字体大小：`font-size:12px;`
- 字体粗细：`font-weight:bold;`
- 字体样式：`font-style:normal;`, normal-正常字体；italic-斜体，用于字体本身就有倾斜的样式；oblique-强制将字体倾斜
- 字体颜色：`color:red;`，十六进制颜色法较为普遍使用`p{color:#00ffff;}`

##### 5. font样式的简写
```css
body{
    font-style:italic;
    font-weight:bold; 
    font-size:12px; 
    line-height:1.5em; 
    font-family:"宋体",sans-serif;
}
/* 缩短为 */
body{
    font:italic  bold  12px/1.5em  "宋体",sans-serif;
}
```
- 使用这一简写方式你至少要指定 font-size 和 font-family 属性，其他的属性(如 font-weight、font-style、font-variant、line-height)如未指定将自动使用默认值。
- 在缩写时 font-size 与 line-height 中间要加入“/”斜扛。

##### 6. text设置
- text-decoration，设置添加到文本的修饰；默认值为none, 定义标准的文本，underline为定义文本下的一条线，overline为定义文本上的一条线，line-through为定义穿过文本下的一条线，一般用于商品折扣价
- text-indent, 首行缩进，`p{text-indent:2em;}` 2em的意思就是文字的2倍大小。
- line-height, 文字行间距， `p{line-height:1.5em;}`
- letter/word-spacing增加或减少字符间的空白, `letter-spacing:50px;`字母与字母之间，`word-spacing:50px;`英文单词与单词之间
- text-align, 文本对齐方式，`text-align:center;`

##### 7. 长度值
1. px像素，是指显示器上的小点
2. em，就是本元素给定字体的 font-size 值，如果元素的 font-size 为 14px ，那么 1em = 14px；如果 font-size 为 18px，那么 1em = 18px
3. 百分比，`p{font-size:12px;line-height:130%}`, 设置行高（行间距）为字体的130%（12 * 1.3 = 15.6px）

### 3. CSS盒模型
##### 1. 元素分类
- 常用的块状元素：`<div>、<p>、<h1>...<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>`
- 常用的内联元素：`<a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>`
- 常用的内联块状元素：`<img>、<input>`

##### 2. 块状元素
1. 每个块级元素都从新的一行开始，并且其后的元素也另起一行。（真霸道，一个块级元素独占一行）
2. 元素的高度、宽度、行高以及顶和底边距都可设置。
3. 元素宽度在不设置的情况下，是它本身父容器的100%（和父元素的宽度一致），除非设定一个宽度。

`display:block`就是将元素显示为块级元素： `a{display:block;}` 

##### 3. 内联元素
1. 和其他元素都在一行上；
2. 元素的高度、宽度及顶部和底部边距不可设置；
3. 元素的宽度就是它包含的文字或图片的宽度，不可改

`display:inline`将元素设置为内联元素: `div{display:inline;}`

##### 4. 内联块状元素
内联块状元素（inline-block）就是同时具备内联元素、块状元素的特点：  
1. 和其他元素都在一行上；
2. 元素的高度、宽度、行高以及顶和底边距都可设置

`display:inline-block`就是将元素设置为内联块状元素

##### 5. none不占据位置
`display: none;` none设置此元素不会被显示，当想要元素隐藏的时候可以使用此值。

##### 6. 盒模型
1. 宽度和高度
盒模型宽度和高度和我们平常所说的物体的宽度和高度理解是不一样的，css内定义的宽（width）和高（height），指的是填充以里的内容范围。   
因此一个元素实际宽度（盒子的宽度）=左边界+左边框+左填充+内容宽度+右填充+右边框+右边界。   
![box](/assets/images/box_width.jpg)   
2. border为盒子添加边框
`border:2px  solid  red;`, 边框样式：dashed（虚线）| dotted（点线）| solid（实线）。   
border-bottom， border-top， border-right， border-left
3. border-radius设置圆角
圆角可分为左上、右上、右下、左下:    
`div{border-radius: 20px 10px 15px 30px;}`   
四个圆角都为10px;可以这么写：   
`div{ border-radius:10px;}`   
4. padding为盒子设置内边距（填充）
元素内容与边框之间是可以设置距离的，称之为“内边距（填充）”。填充也可分为上、右、下、左(顺时针)   
`div{padding:20px 10px 15px 30px;}`   
上、右、下、左的填充都为10px:   
`div{padding:10px;}`
5. margin为盒子设置外边距（边界）
元素与其它元素之间的距离可以使用边界（margin）来设置。边界也是可分为上、右、下、左   
`div{margin:20px 10px 15px 30px;}`   
上右下左的边界都为10px:   
`div{ margin:10px;}`   
上下边界一样为10px，左右一样为20px:   
`div{ margin:10px 20px;}`   
padding和margin的区别，padding在边框里，margin在边框外。

##### 7. 背景色
`background-color:red;`

##### 8. 布局模型
1. 流动模型（Flow）
块状元素都会在所处的包含元素内自上而下按顺序垂直延伸分布，因为在默认状态下，块状元素的宽度都为100%。   
内联元素都会在所处的包含元素内从左到右水平分布显示。（内联元素可不像块状元素这么霸道独占一行）
2. 浮动模型 (Float)
让块状元素并排显示, `div{float:left;}`
3. 层模型（Layer）
对每个图层能够精确定位操作   
绝对定位(position: absolute): 如果想为元素设置层模型中的绝对定位，需要设置position:absolute(表示绝对定位)，这条语句的作用将元素从文档流中拖出来，然后使用left、right、top、bottom属性相对于其最接近的一个具有定位属性的父包含块进行绝对定位。如果不存在这样的包含块，则相对于body元素，即相对于浏览器窗口。   
相对定位(position: relative): 如果想为元素设置层模型中的相对定位，需要设置position:relative（表示相对定位），它通过left、right、top、bottom属性确定元素在正常文档流中的偏移位置。相对定位完成的过程是首先按static(float)方式生成一个元素(并且元素像层一样浮动了起来)，然后相对于以前的位置移动，移动的方向和幅度由left、right、top、bottom属性确定，偏移前的位置保留不动。   
固定定位(position: fixed): 表示固定定位，与absolute定位类型类似，但它的相对移动的坐标是视图（屏幕内的网页窗口）本身。由于视图本身是固定的，它不会随浏览器窗口的滚动条滚动而变化，除非你在屏幕中移动浏览器窗口的屏幕位置，或改变浏览器窗口的显示大小，因此固定定位的元素会始终位于浏览器窗口内视图的某个位置，不会受文档流动影响，这与background-attachment:fixed;属性功能相同   

##### 9. 弹性盒模型
1. flex属性
设置display: flex属性可以把块级元素在一排显示；flex需要添加在父元素上，改变子元素的排列顺序；默认为从左往右依次排列,且和父元素左边没有间隙
2. justify-content属性设置横轴排列方式
`justify-content:flex-start | flex-end | center | space-between | space-around;`属性定义了项目在主轴上的对齐方式。   
flex-start：交叉轴的起点对齐; flex-end：右对齐; center： 居中; space-between：两端对齐，项目之间的间隔都相等; space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
3. align-items属性设置纵轴排列方式
`align-items: flex-start | flex-end | center | baseline | stretch;`   
flex-start：默认值，左对齐; flex-end：交叉轴的终点对齐; center：交叉轴的中点对齐; baseline：项目的第一行文字的基线对齐; stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
4. 给子元素设置flex占比
`flex:1;`, 给子元素设置flex属性,可以设置子元素相对于父元素的占比, flex属性的值只能是正整数,表示占比多少, 给子元素设置了flex之后,其宽度属性会失效

##### 10. 水平居中
- 行内元素：text-align:center
- 块状元素：设置“左右margin”值为“auto”来实现居中的，`margin:20px auto;`

### 4. CSS选择器
- body
- 标签选择器，p{css样式代码}
- 类选择器，.类选器名称{css样式代码;}, 可以使用类选择器词列表方法为一个元素同时设置多个样式。
- ID选择器，#id{css样式代码;}
- 子选择器，大于符号(>),用于选择指定标签元素的第一代子元素, `.food>li{border:1px solid red;}`使class名为food下的子元素li加入红色实线边框
- 后代选择器， 加入空格,用于选择指定标签元素下的后辈元素, `.first  span{color:red;}`, `>`作用于元素的第一代后代，`空格`作用于元素的所有后代。
- 通用选择器，一个（`*`）号指定，它的作用是匹配html中所有标签元素, `* {color:red;}`
- 伪选择器，它允许给html不存在的标签（标签的某种状态）设置样式，`a:hover{color:red;}`为 a 标签鼠标滑过的状态设置字体颜色变红
- 分组选择器，html中多个标签元素设置同一个样式时，可以使用分组选择符（`，`）， `h1,span{color:red;}`

选择器的优先级：内联样式 > id选择器 > 类选择器 > 标签选择器 > 通配符选择器

##### 1. 样式的继承
CSS的某些样式是具有继承性的, 继承是一种规则，它允许样式不仅应用于某个特定html标签元素，而且应用于其后代。如，颜色可以继承，但是边框没法继承

##### 2. 权值计算
浏览器是根据权值来判断使用哪种css样式的，权值高的就使用哪种css样式。   
标签的权值为1，类选择符的权值为10，ID选择符的权值最高为100。   
继承也有权值但很低，有的文献提出它只有0.1，所以可以理解为继承的权值最低
```css
p{color:red;} /*权值为1*/
p span{color:green;} /*权值为1+1=2*/
.warning{color:white;} /*权值为10*/
p span.warning{color:purple;} /*权值为1+1+10=12*/
#footer .note p{color:yellow;} /*权值为100+10+1=111*/
```

##### 3. 选择器最高层级!important
```css
p{color:red!important;}
p{color:green;}
<p class="first">三年级时，我还是一个<span>胆小如鼠</span>的小女孩。</p>
```
这时 p 段落中的文本会显示的red红色。

##### 4. 属性选择器
![选择器](/assets/images/xuanzeqi.jpg)   
```html
<a href="xxx.pdf">我链接的是PDF文件</a>
<a href="#" class="icon">我类名是icon</a>
<a href="#" title="我的title是more">我的title是more</a>
```
```css
a[class="icon"]{
  background: green;
  color:#fff;
}
a[class^=icon]{
  background: green;
  color:#fff;
}
a[href$=pdf]{
  background: orange;
  color: #fff;
}
a[title*=more]{
  background: blue;
  color: #fff;
}
```

##### 5. 结构性伪类选择器
1. `:root{background:orange}` 跟选择器，等同于<html>元素
2. `:not` 否定选择器
```css
input:not([type="submit"]){
  border:1px solid red;
}
```
3. `:empty` 空选择器，用来选择没有任何内容的元素，这里没有内容指的是一点内容都没有，哪怕是一个空格。
```css
p:empty {
  display: none;
}​
```
4. `:target` 目标选择器，用来匹配文档(页面)的url的某个标志符的目标元素
```html
<h2><a href="#brand">Brand</a></h2>
<div class="menuSection" id="brand">
  content for Brand
</div>
<h2><a href="#jake">Brand</a></h2>
<div class="menuSection" id="jake">
 content for jake
</div>
<h2><a href="#aron">Brand</a></h2>
<div class="menuSection" id="aron">
    content for aron
</div>
```
```css
#brand:target {
  background: orange;
  color: #fff;
}
#jake:target {
  background: blue;
  color: #fff;
}
#aron:target {
  background: red;
  color: #fff;
}
```
5. `first-child` 选择父元素的第一个子元素的元素E
```css
ul > li:first-child {
 color: red;
}
```
6. `last-child` 选择的是元素的最后一个子元素
```css
ul>li:last-child{
  background:blue;
}
```
7. `:nth-child(n)`选择器用来定位某个父元素的一个或多个特定的子元素。
其中“n”是其参数，而且可以是整数值(1,2,3,4)，也可以是表达式(2n+1、-n+5)和关键词(odd、even)，但参数n的起始值始终是1，而不是0
```css
ol > li:nth-child(2n){
  background: orange;
}
```
8. `:nth-last-child(n)`从某父元素的最后一个子元素开始计算，来选择特定的元素。
9. `:first-of-type` 指定了元素的类型,其主要用来定位一个父元素下的某个类型的第一个子元素。
```css
.wrapper > p:first-of-type {
  background: orange;
}
```
通过“:first-of-type”选择器，定位div容器中的第一个p元素（p不一定是容器中的第一个子元素），并设置其背景色为橙色。
10. `:nth-of-type(n)`计算父元素中指定的某种类型的子元素
```css
.wrapper > p:nth-of-type(2n){
  background: orange;
}
```
11. `:last-of-type`选择父元素下的某个类型的最后一个子元素。
```css
 .wrapper > p:last-of-type{
  background: orange;
}
```
12. `:nth-last-of-type(n)`选择父元素中指定的某种子元素类型，但它的起始方向是从最后一个子元素开始
```css
.wrapper > p:nth-last-of-type(3){
  background: orange;
}
```
13. `:only-child`选择器选择的是父元素中只有一个子元素，而且只有唯一的一个子元素;匹配的元素的父元素中仅有一个子元素，而且是一个唯一的子元素。
```css
.post p:only-child {
  background: orange;
}
```
14. `:only-of-type`是表示一个元素他有很多个子元素，而其中只有一种类型的子元素是唯一的
```css
.wrapper > div:only-of-type {
  background: orange;
}
```
15. `:enabled`和`:disabled`
```css
input[type="text"]:enabled {
  background: #ccc;
  border: 2px solid red;
}

input[type="text"]:disabled {
  background: rgba(0,0,0,.15);
  border: 1px solid rgba(0,0,0,.15);
  color: rgba(0,0,0,.15);
}
```
16. `:checked`单选按钮和复选按钮选中状态
```css
input[type="checkbox"]:checked + span {
  opacity: 1;
}
```
17. `::selection` 用来匹配突出显示的文本(用鼠标选择文本时的文本)。浏览器默认情况下，用鼠标选择网页文本是以“深蓝的背景，白色的字体”显示的
```css
/* E9+、Opera、Google Chrome 以及 Safari 中支持 ::selection 选择器。
Firefox 支持替代的 ::-moz-selection。 */
::-moz-selection {
  background: red;
  color: green;
}
::selection {
  background: red;
  color: green;
}
```
18. `:read-only` 指定处于只读状态元素的样式
```css
input[type="text"]:-moz-read-only{
  border-color: #ccc;
}
input[type="text"]:read-only{
  border-color: #ccc;
}
```
19. `:read-write` 用来指定当元素处于非只读状态时的样式
```css
input[type="text"]:-moz-read-write{
  border-color: #f36;
}
input[type="text"]:read-write{
  border-color: #f36;
}
```
20. `::before, ::after`给元素的前面或后面插入内容，这两个常和"content"配合使用，使用的场景最多的就是清除浮动
```css
.clearfix::before,
.clearfix::after {
    content: ".";
    display: block;
    height: 0;
    visibility: hidden;
}
.clearfix:after {clear: both;}
.clearfix {zoom: 1;}
```

### 5. Grid布局
`display:grid`   
##### 1. 基本概念
- 网格容器，是其所有网格项的父元素
- 网格项，是网格容器的子元素
- 网格线，组成网格项的分界线
- 网格轨道，两个相邻的网格线之间
- 网格单元，两个相邻的列网格线和两个相邻的行网格线组成
- 网格区域，四个网格线包围的总空间
- fr(单位)，剩余空间分配数；用于在一系列长度值中分配剩余空间，如果多个已指定了多个部分，则剩下的空间根据各自的数字按比例分配
- gr(单位)，网格数

##### 2. display
`display:grid | inline-grid | subgrid;`   
- grid: 生成块级网格
- inline-grid：生成行内网格
- subgrid： 如果网格容器本身是网格项（嵌套网格容器），此属性用来继承其父网格容器的列、行大小

网格布局只作用于子元素；当元素设置了网格布局，column、float、clear、vertical-align属性无效   

##### 3. grid-template
grid-template-columns/grid-template-rows 使用以空格分隔的多个值来定义网格的列和行   
```css
.container{
  grid-template-columns: <track-size> ... | <line-name> <track-size> ...;
  grid-template-rows:<track-size> ... | <line-name> <track-size> ...;
}
```
grid-template-rows在开发过程中，更多用auto   
track-size（轨道大小）：可以使用css长度（px，em等）、百分比或分数（fr）
line-name（网格线名字）：可以选择任何名字   

grid-template-areas，通过引用grid-area属性指定的网格区域的名称来定义网格模板   
```css
.container {
  grid-template-areas:none|
    "grid-area-name|. grid-area-name|. grid-area-name|. ..."
    "grid-area-name|. grid-area-name|. grid-area-name|. ..."
    "...";
}
```
grid-area-name: 使用grid-area属性设置的网格区域的名称   
"."：点号代表一个空网络单元   
none：没有定义网格区域   
![grid-template-area](/assets/images/grid-template-areas.PNG)

##### 4. gap
grid-column-gap/grid-row-gap指定网格线的大小，网格轨道之间的间隙   
```css
.container {
  grid-column-gap: <line-size>;
  grid-row-gap: <line-size>;
  /* 简写 */
  grid-gap|gap: <grid-row-gap> <grid-column-gap>;
}
```

##### 5. items网格项
`justify-tems:start | end | center | stretch;` 沿着行轴对齐网格内的内容, stretch内容快读占据整个网格内容空间（默认值）    
`align-tems:start | end | center | stretch;` 沿着列轴对齐网格内的内容   
`place-items: <align-tems> <justify-tems>` 简写，只写一个对行列都起作用

##### 6. content
`justify-content: start|end|center|stretch|space-around|space-between|space-evenly` 设置网格容器内的网格沿着行轴对齐网格的对齐方式   
`align-content: start|end|center|stretch|space-around|space-between|space-evenly`   
`place-content: <align-content> <justify-content>`

##### 7. grid-auto隐式网格轨道
grid-auto-columns/grid-auto-rows 指自动生成的网格轨道的大小   
`grid-auto-flow:row|column|row dense|column dense;`控制自动布局算法的工作方式，dense告诉自动布局算法，如果后面出现较小的grid item，则尝试在网格中填充空洞

##### 8. 网格项上的属性
```css
/* 使用特定的网格线确定grid item在网格内的位置 */
.item {
  grid-column-start: <number>|<name>|span <number>|span <name>|auto; /* span number 跨越几个网格项*/
  grid-column-end: <number>|<name>|span <number>|span <name>|auto;
  grid-row-start: <number>|<name>|span <number>|span <name>|auto;
  grid-row-end: <number>|<name>|span <number>|span <name>|auto;
}
/* 简写 */
.item {
  grid-column:<start-line> / <end-line> | <start-line> / span <value>;
  grid-row:<start-line> / <end-line> | <start-line> / span <value>;
}

.item {
  /* 定义name， 与grid-template-areas中的名字联合使用 */
  grid-area: <name> | <row-start> / <columen-start> / <row-end> / <column-end>;
}

/* 网格项的对齐方式 */
.item {
  justify-self: <start> | <end> | <center> | <stretch>;
  align-self: <start> | <end> | <center> | <stretch>;
}
```

### 5. CSS函数
##### 1. repeat
repeat(repeat, values), 跟踪列表的重复片段，允许大量显示重复模式的列或行以更紧凑的形式编写   

##### 2. fit-content
`fit-content([<length>|<percentage>])`根据公式min（最大大小、最大值（最小大小、参数））将给定大小夹紧为可用大小

##### 3. minmax
minmax(300px, 50px)取最大值

----


