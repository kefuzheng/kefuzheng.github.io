---
title: VS Code插件基础知识梳理
key: 2020-08-15
tags: vscode
---

### 1. 预备知识
##### 1. 框架
VS Code，便是建立在浏览器开源项目Chromium演化而来——名为Electron的客户端技术之上，它使用了性能极高的JS编译器和浏览器界面技术，将前端的三大编程基础JavaScript，HTML，CSS无缝地衔接进来，并融合了系统层级的编程接口。而VS Code在此之上进一步封装和优化，将编辑器进程和插件进程独立开来，同时高度封装DOM（文档对象模型）接口，禁止用户直接修改界面，我们只能通过VS Code提供的模式去开发符合规范的插件，虽然这些规定限制了开发者的手脚，但是更带来了安全、稳健、性能上的优势。
##### 2. TypeScript
TypeScript起初是微软开发的以JavaScript为基础的编程语言，他兼容JavaScript的所有特性，并扩展了JS的类型系统，使得用户在大型系统开发中更加游刃有余，VS Code天然支持TypeScript，帮助开发者写出更加稳定、安全的代码。因此所有文档的示例，包括插件本身，绝大部分都是使用TypeScript开发的，俗话说“不积跬步无以至千里”，当你足够了插件的基础之后，阅读文档才会更加顺利。  
VS Code已经禁用了CSS和HTML
### 2. 开发环境
vscode, node.js, git, Yeoman, VS Code Extension Generator   
安装: npm install -g yo generator-code  
linux若报权限问题：chown root /root/.config/configstore/（相应的目录）  
在window下用dos安装，用powershell会有意料之外的问题

### 3. 插件开发
##### 1. 第一个插件
```shell
yo code

# ? What type of extension do you want to create? New Extension (TypeScript)
# ? What's the name of your extension? HelloWorld
### Press <Enter> to choose default for all options below ###

# ? What's the identifier of your extension? helloworld
# ? What's the description of your extension? LEAVE BLANK
# ? Enable stricter TypeScript checking in 'tsconfig.json'? Yes
# ? Setup linting using 'tslint'? Yes
# ? Initialize a git repository? Yes
# ? Which package manager to use? npm

code ./helloworld
```
##### 2. 插件目录结构
```text
.
├── .vscode
│   ├── launch.json     // 插件加载和调试的配置
│   └── tasks.json      // 配置TypeScript编译任务
├── .gitignore          // 忽略构建输出和node_modules文件
├── README.md           // 一个友好的插件文档
├── src
│   └── extension.ts    // 插件源代码
├── package.json        // 插件配置清单
├── tsconfig.json       // TypeScript配置
```
##### 3. 插件清单(package.json)
每个VS Code插件都必须包含一个package.json，它就是插件的配置清单。package.json混合了Node.js字段，如：scripts、dependencies，还加入了一些VS Code独有的字段，如：publisher、activationEvents、contributes等。关于这些VS Code字段说明都在插件清单参考中可以找到。  
1. name 和 publisher: VS Code 使用<publisher>.<name>作为一个插件的ID。你可以这么理解，Hello World 例子的 ID 就是vscode-samples.helloworld-sample。VS Code 使用 ID 来区分各个不同的插件。
main: 插件的主入口。
2. activationEvents 和 contributes: 激活事件 and 发布内容配置。
3. engines.vscode: 描述了这个插件依赖的最低VS Code API版本。
4. postinstall 脚本: 如果你的engines.vscode声明的是1.25版的VS Code API，那它就会按照这个声明去安装目标版本。一旦vscode.d.ts文件存在于node_modules/vscode/vscode.d.ts，IntelliSense就会开始运作，你就可以对所有VS Code API进行定义跳转或者语法检查了。
##### 4. 插件入口文件(extension.ts)
插件入口文件会导出两个函数，activate 和 deactivate，你注册的激活事件被触发之时执行activate，deactivate则提供了插件关闭前执行清理工作的机会。  
vscode模块包含了一个位于node ./node_modules/vscode/bin/install的脚本，这个脚本会拉取package.json中engines.vscode字段定义的VS Code API。这个脚本执行过后，你就得到了智能代码提示，定义跳转等TS特性了。
### 4. 扩展点
- contributes.commands  命令
- contributes.keybindings  快捷键
- contributes.menus  菜单
- contributes.viewsContainers 视图容器
- contributes.views  panel
- contributes.snippets  补全块
- contributes.languages 语言支持

### 5. 激活事件
- activationEvents.onCommand  命令
- activationEvents.onView  panel

### 6. 本地打包
- 安装`npm i vsce -g`
- 打包成vsix文件`vsce package`
- 安装扩展，从扩展的右上角选择Install from VSIX安装


----

[vscode docs](https://code.visualstudio.com/api)  
[vscode extension example](https://github.com/microsoft/vscode-extension-samples)  
[VSCode插件开发全攻略（一）概览](https://www.cnblogs.com/liuxianan/p/vscode-plugin-overview.html)   
