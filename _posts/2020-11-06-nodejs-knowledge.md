---
title: node基础知识
key: 2020-11-06
tags: node
---

### 1. npm
##### 1. npm更新
`npm install npm@latest -g`   
`npm install npm@next -g`
##### 2. 升级node到指定版本
1. npm cache clean -f //清除npm缓存
2. npm install -g n //n模块是专门用来管理nodejs的版本
3. n stable // 把当前系统的 Node 更新成最新的 “稳定版本”
4. n lts // 长期支持版 
5. n latest // 最新版
6. n 10.14.2 // 指定安装版本

##### 3. npm包管理工具
- npm init //初始化，在项目中创建一个package.json文件
- npm install/i xxx // 安装到项目目录下
- npm install -g moduleName // g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。
- npm install/i //安装全部依赖
- npm install -save moduleName //-save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。
- npm install -save-dev moduleName //-save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。
- nmp uninstall/un xxx //卸载
- npm update xxx //更新
- npm cache clean --force //清除缓存

cnpm国内版npm   
`npm install -g cnpm -registry=https://registry.npm.taobao.org`  

##### 4. npx
npx是一个npm的命令行工具，它可以让您方便地运行本地或远程的npm包，而无需全局安装或指定路径。

### 2. nvm管理nodejs版本
nvm：Node Version Manager
- nvm install stable  安装最新稳定版 node
- nvm list  列出所有安装的版本
- nvm install <version>  安装指定版本
- nvm use <version>  切换使用指定的版本node
- nvm uninstall <version>  删除已安装的指定版本
- nvm alias default <version>  设置默认版本

### 3. yarn
yarn和npm都是JavaScript/TypeScript的包管理器，可以用来安装、更新、删除和发布包。Yarn的优点是可以方便地共享代码、统一版本控制、简化依赖管理、提高安装速度等。
##### 1. yarn和npm区别
- yarn使用yarn.lock文件来锁定每个包的版本和校验和，保证了安装的一致性和安全性。npm使用package-lock.json或npm-shrinkwrap.json文件来锁定版本，但不包含校验
- yarn可以并行地下载和安装包，提高了安装的速度和效率。npm是按照队列顺序来安装包的，速度相对较慢。
- yarn可以在离线模式下安装本地缓存的包，不需要互联网连接。npm必须要有互联网连接才能安装包。
- yarn提供了一些额外的命令和功能，如yarn licenses、yarn why、yarn workspaces等，可以方便地管理包的许可证、依赖关系和工作区。npm没有完全对应的命令和功能。12
- yarn和npm的命令语法有一些差异

##### 2. 安装yarn
`npm install -g yarn`

##### 3. yarn常用命令
- 初始化新项目：`yarn init`，通过交互式会话创建一个package.json文件。
- 添加依赖包：`yarn add [package]`，通过yarn add添加依赖会更新package.json以及yarn.lock文件。
- 升级依赖包：`yarn upgrade [package]`，更新指定的包或所有包。
- 移除依赖包：`yarn remove [package]`，删除包，会更新package.json和yarn.lock文件。
- 安装项目的全部依赖：`yarn或yarn install`，安装所有依赖，可以使用--force标志强制下载安装。
- 运行脚本：`yarn run [script]`，用来执行package.json中scripts属性下定义的脚本。
- 显示某个包信息：`yarn info [package]`，拉取包的信息并返回为树格式。
- 列出项目的所有依赖：`yarn list`，列出当前工作文件夹所有的依赖。
- 管理yarn配置文件：`yarn config`，设置、读取、删除或查看当前配置。
- 缓存：`yarn cache`，列出、清除或打印出当前yarn全局缓存的位置。

##### 4. yarn workspaces
各个库之间存在依赖，如 A 依赖于 B，因此我们通常需要将 B link 到 A 的 node_module 里，一旦仓库很多的话，手动的管理这些 link 操作负担很大，因此需要自动化的 link 操作，按照拓扑排序将各个依赖进行 link。启用 yarn workspaces 可以把所有的依赖提升到顶层的 node_modules 中，并且在 node_modules 中链接到本地的 package，自动的帮忙解决安装和 link 问题。   
yarn workspace可以让您在一个项目中管理多个包，同时简化跨引用和依赖安装。要使用yarn workspace，您需要在项目的根目录下的package.json文件中添加一个workspaces数组，列出指向您的包文件夹的相对通配符模式。

### 4. lerna
##### 1. Mono repo
Mono repo是一种软件开发策略，指的是将多个相关或不相关的项目的代码存储在同一个仓库中。Mono repo的优点是可以方便地共享代码、统一版本控制、简化依赖管理等。Mono repo的缺点是可能导致仓库过大、构建速度慢、合并冲突多等。

##### 2. lerna
Lerna是一种用于管理和发布多个JavaScript/TypeScript包的快速、现代的构建系统。它是使用Mono repo策略的最佳工具，具有分布式缓存、目标依赖和交互式工作空间可视化等功能。

##### 3. lerna常用命令
`npx lerna init --dryRun` # Initialize lerna (using --dryRun to preview the changes)   
`npx lerna init --packages="packages/*"` # Passing a single pattern

##### 4. lerna + yarn workspace
在 lerna.json 中声明使用 yarn workspace 进行依赖管理   
```json
{
  ...
  "npmClient": "yarn",
  "useWorkspaces": true
}
```
在 root/package.json 下必需包含 workspaces 数组，与 lerna.json 下的 packages 保持一致   
```json
{
  "private": true, // 工作空间不需要发布
  ...
  "workspaces": ["packages/*"]
}
```
yarn install 会将 package 下的依赖统一安装到根目录之下。这有利于提升依赖的安装效率和不同 package 间的版本复用（有些包是需要私有依赖的，而私有依赖会被多个包安装多次，而提升依赖可以解决这一问题）。   
yarn install 会自动帮助解决安装（包括根目录下的安装）和 packages link 问题。

### 5. npm安装依赖(dependency)版本号及更新规则
##### 1. 版本号格式
{major}.{minor}.{patch}-{pre-release}   
{主版本号}.{次版本号}.{修补版本号}-{非正式版版本号}    
```json
"dependencies": {
    "typescript": "^4.0.5-0",
}
// 主版本号: 4
// 次版本号: 0
// 修补版本号: 5
// 非正式版版本号: 0
```
##### 2. dependency 版本更新规则
![dependency](/assets/images/node-dependency.jpg)   

### 6. yarn.lock 的作用
锁定唯一版本，package.json 里定义的是版本区间，如^1.0.0，而 yarn.lock 里的version字段是唯一的版本号，如1.0.0
```
core-js-compat@^3.0.0:
  version "3.14.0"
  resolved "https://registry.npmjs.org/core-js-compat/-/core-js-compat-3.14.0.tgz#b574dabf29184681d5b16357bd33d104df3d29a5"
  integrity sha1-tXTavykYRoHVsWNXvTPRBN89KaU=
  dependencies:
    browserslist "^4.16.6"
    semver "7.0.0"
```
- identifier(s)
第一行的core-js-compat@^3.0.0是依赖的 identifier。和 package.json 里对应的包名和版本区间，用@连接。这边的标题里带了(s)，是因为多个 identifier 最终可能都指向同一个版本（具体例子可以看下文### dependencies里给出的例子）
- version
第二行version是实际安装的版本。通常是满足版本区间里的一个版本，比如上一行 identifier 里版本区间是^3.0.0，这里实际安装的是3.14.0，符合要求。但是为什么要说是“通常”呢，因为有例外，在后文### resolutions部分会讲到。
- resolved
第三行resolved的是一个链接，是下载这个包的地址。这个 url 里的域名部分跟项目里配置的 .npmrc或你本地的 npm 配置的 registry有关。
- integrity
第四行integrity是对resolved下载下来的文件进行完整性校验。如果出现 diff，说明同一个下载链接对应的文件被修改过。
- dependencies
第五行dependencies是这个包自己的依赖。如这里依赖的browserslist "^4.16.6"，你想看下实际安装的哪个版本，就可以把它拼成 Identifierbrowserslist@^4.16.6"，以此为关键字在 yarn.lock 中搜索，就能找到对应的“块”了。

### 7. Nodejs 处理异步（获取异步数据并处理）的方法
回调函数方式: 将异步方法如readFile封装到一个自定义函数中，通过将异步方法得到的结果传给自定义方法的回调函数参数。
```
//封装
var func = function(filePath,callback){
    fs.readFile(filePath, function(err, data){
        if(err){
            return false;
        }
        callback(data);
    }) 
}

//调用
func('./a.txt', function(res){
    //处理异步方法返回得到的数据
    console.log(res);
})
```

### 8. 创建typescript工程
##### 1. 创建工程
```ts
// 1. 创建项目目录
mkdir my-typescript-project
cd my-typescript-project

// 2. 初始化 Git 仓库（可选）
git init

// 3. 初始化 npm 环境：
npm init -y

// 4. 安装 TypeScript
npm install typescript --save-dev

// 5. 创建 TypeScript 文件
// app.ts
function greet(name: string) {
    console.log(`Hello, ${name}!`);
}

greet('TypeScript');

// 6. 编译 TypeScript 文件：
npx tsc

// 7. 运行项目：
node app.js
```

##### 2. vscode debug 工程
```ts
// 初始化 tsconfig.json 文件
npx tsc --init

// 设置"sourceMap": true

// 配置launch.json 文件
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceFolder}/app.ts",
            "preLaunchTask": "tsc: build - tsconfig.json",
            "outFiles": ["${workspaceFolder}/out/**/*.js"]
        }
    ]
}
```

----

[lerna + yarn workspace 使用总结](https://juejin.cn/post/7097820725301477406)  
[NPM install -save 和 -save-dev 傻傻分不清](https://www.cnblogs.com/limitcode/p/7906447.html)   
[yarn.lock 你锁明白了吗？](https://zhuanlan.zhihu.com/p/400193691)   
