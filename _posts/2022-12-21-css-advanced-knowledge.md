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

----

[scss实现样式复用: 继承、占位符、混合宏](https://cloud.tencent.com/developer/article/1617088)  
[NPM install -save 和 -save-dev 傻傻分不清](https://www.cnblogs.com/limitcode/p/7906447.html)  
