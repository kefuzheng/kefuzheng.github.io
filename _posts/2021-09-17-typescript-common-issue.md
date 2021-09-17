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
### 5. 数组concat()
```typescript
var alpha =  [ “a” ， “b” ， “c” ];  
VAR num =  [ 1 ， 2 ， 3 ];

var alphaNumeric = alpha.concat(num);  
```


----

[TypeScript小状况之遍历对象属性](https://juejin.cn/post/6844904147146260488)   