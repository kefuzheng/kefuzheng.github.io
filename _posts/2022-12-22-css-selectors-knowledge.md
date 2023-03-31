---
title: CSS选择器
key: 2022-12-22
tags: HTML+CSS
---

### 1. CSS选择器
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

### 2. 权值计算
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

### 3. 选择器最高层级!important
```css
p{color:red!important;}
p{color:green;}
<p class="first">三年级时，我还是一个<span>胆小如鼠</span>的小女孩。</p>
```
这时 p 段落中的文本会显示的red红色。

### 4. 属性选择器
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

### 5. 结构性伪类选择器
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
```xml
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