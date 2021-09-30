---
title: Typescript常见问题
key: 2020-08-10
tags: typescript
---

### 1. 遍历对象属性
遍历对象属性会报错
```typescript
function test (foo: object) {
  for (let key in foo) {
    console.log((foo as any)[key]); // 报错消失
    // do something
  }
}
```
### 2. 型判断typeof,instanceof，以及as断言
```typescript
// 基本类型用typeof, string,number,object,null,undefined
console.log(typeof str == 'string')
console.log(typeof b1)

// 引用类型，接口，类类型，Array和RegExp 用instanceof
console.log(a instanceof Achild);
console.log([] instanceof Array;
console.log({} instanceof Object);
console.log(/\d/ instanceof RegExp);

// 断言，as和<>的用法
console.log((p1 as String).length);
console.log((<String>p1).length);
```

### 3. parse json
```typescript
const data = `{
    "name": "Alex",
    "age": 20,
    "grade": "A",
    "marks": [
        {"sub1" : 80},
        {"sub2" : 30}
    ]
}`;

let json = JSON.parse(data);
console.log(json);

console.log(`name: ${json.name} sub1: ${json.marks[0].sub1} sub2: ${json.marks[1].sub2}`);
```

### 4. 判断文件是否存在
```typescript
import * as fs from 'fs';

const file = 'text.txt';

// 同步的方法
fs.existsSync(file)

// 检查文件是否存在于当前目录中。
fs.access(file, fs.constants.F_OK, (err) => {
  console.log(`${file} ${err ? '不存在' : '存在'}`);
});

// 检查文件是否可读。
fs.access(file, fs.constants.R_OK, (err) => {
  console.log(`${file} ${err ? '不可读' : '可读'}`);
});

// 检查文件是否可写。
fs.access(file, fs.constants.W_OK, (err) => {
  console.log(`${file} ${err ? '不可写' : '可写'}`);
});

// 检查文件是否存在于当前目录中、以及是否可写。
fs.access(file, fs.constants.F_OK | fs.constants.W_OK, (err) => {
  if (err) {
    console.error(
      `${file} ${err.code === 'ENOENT' ? '不存在' : '只可读'}`);
  } else {
    console.log(`${file} 存在，且可写`);
  }
});
```
### 5. path模块
```typescript
import * as path from 'path'

path.dirname(p) // 获取指定路径中目录名，如果当前为目录，则返回上一级目录

path.basename(p,[ext]) //获取路径中文件名,后缀是可选的，如果加，请使用'.ext'方式来匹配，则返回值中不包括后缀名

path.resolve(path1, [path2]..[pathn]) //获取绝对路径

path.relative(from, to) //获取相对路径

path.join([...paths]) //将给定的路径拼接起来，如果给定的路径长度为零，则返回”.“,表示当前的目录
```
### 6. 数组concat()
```typescript
var alpha =  [ “a” ， “b” ， “c” ];  
VAR num =  [ 1 ， 2 ， 3 ];

var alphaNumeric = alpha.concat(num);  
```

### 7. 解析xml
```typescript
// install xml2js
npm i --save xml2js

import * as xml2js from 'xml2js'
import * as fs from 'fs';

let data = fs.readFileSync(uri.fsPath);
xml2js.parseString(data, async function (err, result) {
    if (err) {
        console.log(err);
        return;
    }
    result.element...
})

```

### 8. 正则表达式
##### 1. 创建正则表达式
1. 字面量创建方式：let reg = /pattern/flags
2. 构造函数实例创建方式：let reg = new RegExp(pattern, flags)

- pattern 正则表达式
- flags 标识（修饰符）
- 修饰符 i 忽略大小写匹配
- 修饰符 m 多行匹配（即在到达一行文本末尾时还会继续寻找下一行中是否与正则匹配的项）
- 修饰符 g 全局匹配（模式应用于所有的字符串），而非在找到第一个匹配项时停止

```typescript
let reg1 = new RegExp('\d'); // 表示的 d (如果需要\,必须转义\\)
let reg2 = new RegExp('\\d'); // 表示的 \d
let reg3 = /\d/; // 表示的 
```
##### 2. 元字符
特殊含义的元字符：   
- \d : 0-9之间的任意一个数字  \d只占一个位置
- \w : 数字，字母 ，下划线 0-9 a-z A-Z _
- \s : 空格或者空白等
- \D : 除了\d
- \W : 除了\w
- \S : 除了\s
- . : 除了\n之外的任意一个字符
- \ : 转义字符
- | : 或者
- () : 分组
- \n : 匹配换行符
- \b : 匹配边界 字符串的开头和结尾 空格的两边都是边界 => 不占用字符串位数
- ^ : 限定开始位置 => 本身不占位置
- $ : 限定结束位置 => 本身不占位置
- [a-z] : 任意字母 []中的表示任意一个都可以
- [^a-z] : 非字母 []中^代表除了
- [abc] : abc三个字母中的任何一个 [^abc]除了这三个字母中的任何一个字符

次数的量词元字符   
- * : 0到多个
- + : 1到多个
- ? : 0次或1次 可有可无
- {n} : 正好n次；
- {n,} : n到多次
- {n,m} : n次到m次

##### 3. 方法
- reg.test(str)  //用来验证字符串是否符合正则 符合返回true 否则返回false
```typescript
var str = 'hello';
var reg = /\w+/; // 模式：匹配1到多个的数字、字母、下划线
console.log(reg.test(str)); // true
```
- reg.exec() //用来捕获符合规则的字符串
```typescript
var str = 'tom287aaa879bbb666';
var reg = /\d+/;
console.log(reg.exec(str));
// 返回数组中 [0:"287", index:3, input:"tom287aaa879bbb666"]
// 0 : "123" 表示我们捕获到的字符串
// index: 3 表示捕获开始位置的索引
// input: tom287aaa879bbb666 表示原有的字符串

标识符 g 作用
var str = 'abc123cba456aaa789';
var reg = /\d+/g;  //此时加了标识符g
console.log(reg.lastIndex) // lastIndex : 0
console.log(reg.exec(str)) //  ["123", index: 3, input: "abc123cba456aaa789"]
console.log(reg.lastIndex) // lastIndex : 6
console.log(reg.exec(str)) // ["456", index: 9, input: "abc123cba456aaa789"]
console.log(reg.lastIndex) // lastIndex : 12
console.log(reg.exec(str)) // ["789", index: 15, input: "abc123cba456aaa789"]
console.log(reg.lastIndex) // lastIndex : 18
console.log(reg.exec(str)) // null
console.log(reg.lastIndex) // lastIndex : 0

注意：
// 每次调用exec方法时,捕获到的字符串都不相同
// lastIndex ：这个属性记录的就是下一次捕获从哪个索引开始。
// 当未开始捕获时，这个值为0。
// 如果当前次捕获结果为null。那么lastIndex的值会被修改为0.下次从头开始捕获。
// 而且这个lastIndex属性还支持人为赋值。
```
- str.match(reg) 如果匹配成功，就返回匹配成功的数组，如果匹配不成功，就返回null
```typescript
// 如果正则表达式没有标识符 g 则exec与match功能一样的
// 如果正则表达式有标识符 g 则exec与match有明显区别
// 当全局匹配时，match方法会一次性把符合匹配条件的字符串全部捕获到数组中,
// 如果想用exec来达到同样的效果需要执行多次exec方法。
var str = 'abc123cba456aaa789';
var reg = /\d+/g;
console.log(reg.exec(str)); // ["123", index: 3, input: "abc123cba456aaa789"]
console.log(str.match(reg)); // ["123", "456", "789"]
```
- str.replace(reg,newStr) 正则去匹配字符串，匹配成功的字符去替换成新的字符串

```typescript
var str = 'b123bc456d';
var res = str.replace(/\d/g,'Q')
console.log(res) // "bQQQbcQQQd" 将所有的数字替换字符Q
```
##### 4. 贪婪性 与 懒惰性
```typescript
var str = '123aaa456';
var reg = /\d+/;  //只捕获一次,一次尽可能多的捕获
var res = str.match(reg)
console.log(res)  // ["123", index: 0, input: "123aaa456"]
reg = /\d+?/g; //解决贪婪性、懒惰性
res = str.match(reg)
console.log(res)  // ["1", "2", "3", "4", "5", "6"]
```

----

[TypeScript小状况之遍历对象属性](https://juejin.cn/post/6844904147146260488)   
[node.js中fs模块+path模块](https://blog.csdn.net/WJ930708/article/details/81738256)  
[Nodejs正则表达式概述](https://www.jianshu.com/p/10232b4afedc)  
[Nodejs正则表达式](https://blog.csdn.net/dc_show/article/details/42083377)  
[正则表达式在线测试](https://c.runoob.com/front-end/854/)  
[TypeScript模块(外部模块)、命名空间（内部模块）](https://blog.csdn.net/qq_37708564/article/details/106256723)  
