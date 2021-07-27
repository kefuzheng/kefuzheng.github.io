---
title: nodejs基础知识
key: 2020-11-06
tags: nodejs
---

### 1. 取得当前执行路径
- process.cwd() 当前执行程序的路径（执行命令行时候的路径,不是代码路径 例如 在根目录下执行 node ./xxx/xxx/a.js 则 cwd 返回的是 根目录地址 ）
- __dirname: 代码存放的位置
- process.execPath: 当前执行的node路径（如：/bin/node）


### 2. npm包管理工具
- npm init //初始化
- npm install/i xxx // 安装
- npm install/i //安装全部依赖
- nmp uninstall/un xxx //卸载
- npm update xxx //更新

cnpm国内版npm   
`npm install -g cnpm -registry=https://registry.npm.taobao.org`   

### 3. 模块
##### 1. 全局模块
何时何地都能访问，不需要引用
- process.env //环境变量
- process.argv //入参
- __dirname // 当前路径

##### 2. 系统模块
需要require()， 但不需要单独下载   
`let path = require('path')` //处理文件路径和目录      
`let fs = require('fs')` //用于文件读写操作

##### 3. 自定义模块
require自己封装的模块   
require：如有路径就去路径里面找，没有就去node_modules里面找；既没有路径，也没有node_modules，就去安装目录里找   
```js
exports.a=1;
exports.b=2;

module.exports={
  c=3;
  d=4
}
```

##### 4. HTTP模块
服务器对象：http.createServer()   
```js
let http=require('http')
let fs=require('fs')
http.createServer((req, res)=>{
  console.log(req.url);
  fs.readFile('./${req.url}', (err,data)=>{
    if(err){
      res.writeHead(404);
      res.end('404 not found');
    }else{
      res.end(data);
    }
  })
}).listen(8080)

```


----

[nodejs实现文件的拷贝复制](https://www.cnblogs.com/coding4/p/7495968.html)  
