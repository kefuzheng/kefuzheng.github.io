---
title: css, scss进阶知识
key: 2022-12-21
tags: HTML+CSS
---

### 1. 字体
`font-style: italic`斜体

### 2. input框文本居右
`<input type="text" value="bbbbbbbb"  size="100" style="text-align:right" />`

### 3. @font-face 规则
```css
// 在 @font-face 规则中，您必须首先定义字体名称（例如 myFirstFont），然后指向字体文件。
@font-face {
  font-family: myFirstFont;
  src: url(sansation_light.woff);
}

// 将字体用于 HTML 元素，请通过 font-family 属性引用字体的名称（myFirstFont）
div {
  font-family: myFirstFont;
}
```

### 4. 选择器
##### 1. 属性选择器
- `[attr]` 用于选取带有指定属性的元素
- `[attr=value]`	用于选取带有指定属性和值的元素
- `[attr~=value]`	用于选取属性值中包含指定词汇的元素
- `[attr|=value]`	用于选取带有以指定值开头的属性值的元素，该值必须是整个单词
- `[attr^=value]`	匹配属性值以指定值开头的每个元素。
- `[attr$=value]`	匹配属性值以指定值结尾的每个元素。
- `[attr*=value]`	匹配属性值中包含指定值的每个元素。

例如：   
`div[class^='col-']` 找出class名称为 ‘col-’ 开头的div

### 5. outline(边框) 属性
```css
// outline-color, outline-style, outline-width
p{
  outline:#00FF00 dotted thick;
}
```

##### 2. :focus 选择器
```css
// 获取焦点时，背景色为黄色
input:focus
{ 
  background-color:yellow;
}
```

### 6. div层悬浮在最上方
```css
.t1 {
  width: 200px;
  height: 200px;
  position: absolute; // 一定要设置
  z-index: 999; // 数字越大代表越置前， -1 最底层
}
```

### 7. :root用法
:root是指文档的根元素，在其中定义的变量可作为全局变量   
声明变量的时候，变量名前面要加两根连词线（--）   
var()函数用于读取变量
```css
:root {
  --main-bg-color: brown;
}

.one {
  color: white;
  background-color: var(--main-bg-color);
  margin: 10px;
  width: 50px;
  height: 50px;
  display: inline-block;
}
```

### 8. scss编程语言特性
##### 1. 继承
在 scss 中，一个样式类，可以被其他样式类直接继承，从而减少重复代码的编写。
```scss
.btn {
    border: 1px solid blue;
}

.btn--primary {
    color: black;
    @extend .btn;
}

.btn--info {
    color: gray;
    @extend .btn;
}
```

##### 2. 占位符
顾名思义，如果不被引用，它是不会被编译到 css 文件中的。这是和继承最大区别。
```scss
%btn {
    border: 1px solid blue;
}

// 没有被extend, 不会出现在css文件中
%test-btn {
    border: 1px solid black;
}

.btn--primary {
    color: black;
    @extend %btn;
}

.btn--info {
    color: gray;
    @extend %btn;
}
```

##### 3. 函数（混合宏）
通过关键字@mixin声明，@include引用
```scss
// 无参
@mixin btn {
    border-radius: 3px;
}

.box {
    color: white;
    @include btn;
}

// 有参
$radius: 3px !default;

@mixin btn($radius: 5px) {
    // 默认是 5px
    border-radius: $radius;
}

.box {
    color: white;
    @include btn($radius); // 传入参数
}

```

##### 4. ...参数运算符
```scss
$height: 15px !default;
$width: 18px !default;

@mixin size($list...) {
    @if length($list) == 0 {
        height: $height;
        width: $width;
    } @else if length($list) == 1 {
        height: $list;
        width: $list;
    } @else if length($list) == 2 {
        height: nth($list, 1);
        width: nth($list, 2);
    } @else {
        @debug "Too many parameters";
    }
}

.btn--primary {
    @include size();
}

.btn--big {
    @include size(20px, 25px);
}

.btn--square {
    @include size(18px);
}

.btn--test {
    @include size(
        1px,
        2px,
        3px,
        4px
    ); // just a test. console output "Too many parameters" what we have defined.
}
```

### 9. animation动画属性
```css
div
{
    animation:mymove 5s infinite;
}
```

### 10. border不显示
width：100%+border超过100%，会导致不显示   
宽度100%是不包含margin、border和padding 的

### 11. before/after

```css
/* 每个p元素之前插入内容
使用content属性来指定要插入的内容 */
p:before
{
    content:"Read this: ";
}

/* 在每个链接后的括号内加上网址（href属性）	
将元素的attribute属性以字符串形式返回*/
a:after
{
    content: " (" attr(href) ")";
}
```

### 12. div内文字的溢出部分用省略号显示
```css
/* 单行文字末尾省略号显示 */
.div1{
    height: 100px;
    width:100px;
    background:green; 
    text-overflow: ellipsis;  /* ellipsis:显示省略符号来代表被修剪的文本  string:使用给定的字符串来代表被修剪的文本*/ 
    white-space: nowrap;   /* nowrap:规定段落中的文本不进行换行   */ 
    overflow: hidden; /*超出部分隐藏*/
}
/* 多行文字末尾省略号显示 */
.div2{
    /* height: 100px; */
    width:100px;
    background:yellowgreen; 
    display: -webkit-box;  /*  display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示  */
    -webkit-box-orient: vertical; /*  -webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式  */
    -webkit-line-clamp: 5; /*  -webkit-line-clamp用来限制在一个块元素显示的文本的行数 */
    overflow: hidden;
}
```

### 13. scss中的嵌套原则
##### 1. 允许将一套 CSS 样式嵌套进另一套样式中，内层的样式将它外层的选择器作为父选择器
```scss
#app {
    h1 {
        text-align: center;
    }
}
// 编译结果
#app h1 { text-align: center; } 
```
##### 2. 父选择器 &
 & 代表嵌套规则外层的父选择器, &所在的位置只代表了他的父亲，也就是说在每一层，指向是都是他所在的父元素。
```scss
#main {
    color: black;
    a {
        font-weight: bold;
        &:hover {
            color: red;
        }
    }
}
// 编译结果
#main {
    color: black;
}
#main a {
    font-weight: bold;
}
#main a:hover {
    color: red;
}
```
##### 3. 属性嵌套
有些 CSS 属性遵循相同的命名空间 (namespace)，比如 font-family, font-size, font-weight 都以 font 作为属性的命名空间。为了便于管理这样的属性，同时也为了避免了重复输入，Sass 允许将属性嵌套在命名空间中
```scss
.funky {
    font: {
        family: fantasy;
        size: 30px;
        weight: bold;
    }
}
// 编译结果
.funky {
    font-family: fantasy;
    font-size: 30px;
    font-weight: bold;
}
```

### 14. white-space
hite-space属性用来控制容器的文本中带有空白符、制表符、换行符等的显示
- normal：默认，忽略文本中所有的空白、换行符；只有文本存在 `<br>` 或文本达到框的约束时，文本才会换行
- nowrap：和normal类似，忽略文本中所有的空白、换行符；遇到框的宽度约束时不会自动换行，文本只有在有 `<br>` 时才会换行
- pre：保留文本中的空白、换行符；遇到框的宽度约束时不会自动换行，文本存在 `<br>` 或文本中有换行符时，文本才会换行
- pre-wrap：和pre类似，保留文本中的空白、换行符；文本存在 <br> 或文本中有换行符时，或者遇到框的宽度约束时，文本都才会换行
- pre-line：会略文本中的空白符；文本存在 <br> 或文本中有换行符时，或者遇到框的宽度约束时，文本都才会换行


----

[scss实现样式复用: 继承、占位符、混合宏](https://cloud.tencent.com/developer/article/1617088)  
[NPM install -save 和 -save-dev 傻傻分不清](https://www.cnblogs.com/limitcode/p/7906447.html)  
[css中border不显示如何解决](https://www.yisu.com/zixun/447540.html)  
[div内文字的溢出部分用省略号显示](https://www.cnblogs.com/FHC1994/p/11502929.html)  
[Scss中的嵌套规则](https://blog.csdn.net/weixin_41229588/article/details/106052540)  
