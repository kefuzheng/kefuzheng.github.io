---
title: TypeScript项目中的tsconfig.json配置
key: 2021-09-18
tags: typescript
---


### 1. tsconfig.json
tsconfig.json文件是TypeScript项目的配置文件，它可以指定项目的根文件和编译选项。通过使用tsconfig.json文件，您可以方便地管理和编译多个TypeScript文件，而不需要每次手动输入编译命令和参数。   
在TS的项目中，TS最终都会被编译JS文件执行，TS编译器在编译TS文件的时候都会先在项目根目录的tsconfig.json文件，根据该文件的配置进行编译，默认情况下，如果该文件没有任何配置，TS编译器会默认编译项目目录下所有的.ts、.tsx、.d.ts文件   
如果一个目录下存在一个tsconfig.json文件，那么它意味着这个目录是TypeScript项目的根目录。

### 2. 文件选项配置
- files: 表示编译需要编译的单个文件列表
```xml
"files": [
  // 指定编译文件是src目录下的a.ts文件
  "scr/a.ts"
]
```
- include: 表示编译需要编译的文件或目录
```xml
"include": [
  // "scr" // 会编译src目录下的所有文件，包括子目录
  // "scr/*" // 只会编译scr一级目录下的文件
  "scr/*/*" // 只会编译scr二级目录下的文件
]
```
- exclude：表示编译器需要排除的文件或文件夹
默认排除node_modules文件夹下文件   
```xml
"exclude": [
  // 排除src目录下的lib文件夹下的文件不会编译
  "src/lib"
]
```
extends: 引入其他配置文件，继承配置   
```xml
// 把基础配置抽离成tsconfig.base.json文件，然后引入
"extends": "./tsconfig.base.json"
```
- compileOnSave:设置保存文件的时候自动编译
vscode暂不支持该功能，可以使用'Atom'编辑器, `"compileOnSave": true`

### 3. compilerOptions
```xml
"compilerOptions": {
  "incremental": true, // TS编译器在第一次编译之后会生成一个存储编译信息的文件，第二次编译会在第一次的基础上进行增量编译，可以提高编译的速度
  "tsBuildInfoFile": "./buildFile", // 增量编译文件的存储位置
  "diagnostics": true, // 打印诊断信息 
  "target": "ES5", // 目标语言的版本
  "module": "CommonJS", // 生成代码的模板标准
  "outFile": "./app.js", // 将多个相互依赖的文件生成一个文件，可以用在AMD模块中，即开启时应设置"module": "AMD",
  "lib": ["DOM", "ES2015", "ScriptHost", "ES2019.Array"], // TS需要引用的库，即声明文件，es5 默认引用dom、es5、scripthost,如需要使用es的高级版本特性，通常都需要配置，如es8的数组新特性需要引入"ES2019.Array",
  "allowJS": true, // 允许编译器编译JS，JSX文件
  "checkJs": true, // 允许在JS文件中报错，通常与allowJS一起使用
  "outDir": "./dist", // 指定输出目录
  "rootDir": "./", // 指定输出文件目录(用于输出)，用于控制输出目录结构
  "declaration": true, // 生成声明文件，开启后会自动生成声明文件
  "declarationDir": "./file", // 指定生成声明文件存放目录
  "emitDeclarationOnly": true, // 只生成声明文件，而不会生成js文件
  "sourceMap": true, // 生成目标文件的sourceMap文件
  "inlineSourceMap": true, // 生成目标文件的inline SourceMap，inline SourceMap会包含在生成的js文件中
  "declarationMap": true, // 为声明文件生成sourceMap
  "typeRoots": [], // 声明文件目录，默认时node_modules/@types
  "types": [], // 加载的声明文件包
  "removeComments":true, // 删除注释 
  "noEmit": true, // 不输出文件,即编译后不会生成任何js文件
  "noEmitOnError": true, // 发送错误时不输出任何文件
  "noEmitHelpers": true, // 不生成helper函数，减小体积，需要额外安装，常配合importHelpers一起使用
  "importHelpers": true, // 通过tslib引入helper函数，文件必须是模块
  "downlevelIteration": true, // 降级遍历器实现，如果目标源是es3/5，那么遍历器会有降级的实现
  "strict": true, // 开启所有严格的类型检查
  "alwaysStrict": true, // 在代码中注入'use strict'
  "noImplicitAny": true, // 不允许隐式的any类型
  "strictNullChecks": true, // 不允许把null、undefined赋值给其他类型的变量
  "strictFunctionTypes": true, // 不允许函数参数双向协变
  "strictPropertyInitialization": true, // 类的实例属性必须初始化
  "strictBindCallApply": true, // 严格的bind/call/apply检查
  "noImplicitThis": true, // 不允许this有隐式的any类型
  "noUnusedLocals": true, // 检查只声明、未使用的局部变量(只提示不报错)
  "noUnusedParameters": true, // 检查未使用的函数参数(只提示不报错)
  "noFallthroughCasesInSwitch": true, // 防止switch语句贯穿(即如果没有break语句后面不会执行)
  "noImplicitReturns": true, //每个分支都会有返回值
  "esModuleInterop": true, // 允许export=导出，由import from 导入
  "allowUmdGlobalAccess": true, // 允许在模块中全局变量的方式访问umd模块
  "moduleResolution": "node", // 模块解析策略，ts默认用node的解析策略，即相对的方式导入
  "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
  "paths": { // 路径映射，相对于baseUrl
    // 如使用jq时不想使用默认版本，而需要手动指定版本，可进行如下配置
    "jquery": ["node_modules/jquery/dist/jquery.min.js"]
  },
  "rootDirs": ["src","out"], // 将多个目录放在一个虚拟目录下，用于运行时，即编译后引入文件的位置可能发生变化，这也设置可以虚拟src和out在同一个目录下，不用再去改变路径也不会报错
  "listEmittedFiles": true, // 打印输出文件
  "listFiles": true// 打印编译的文件(包括引用的声明文件)
}
```
### 4. 工程引用配置
- references 指定工程引用依赖
在项目开发中，有时候我们为了方便将前端项目和后端node项目放在同一个目录下开发，两个项目依赖同一个配置文件和通用文件，但我们希望前后端项目进行灵活的分别打包，那么我们可以进行如下配置：
```xml
Project
  - src
    - client //客户端项目
      - index.ts // 客户端项目文件
      - tsconfig.json // 客户端配置文件
        {
          "extends": "../../tsconfig.json", // 继承基础配置
          "compilerOptions": {
            "outDir": "../../dist/client", // 指定输出目录
          },
          "references": [ // 指定依赖的工程
            {"path": "./common"}
          ]
        }
    - common // 前后端通用依赖工程
      - index.ts  // 前后端通用文件
      - tsconfig.json // 前后端通用代码配置文件
        {
          "extends": "../../tsconfig.json", // 继承基础配置
          "compilerOptions": {
            "outDir": "../../dist/client", // 指定输出目录
          }
        }
    - server // 服务端项目
      - index.ts // 服务端项目文件
      - tsconfig.json // 服务端项目配置文件
        {
          "extends": "../../tsconfig.json", // 继承基础配置
          "compilerOptions": {
            "outDir": "../../dist/server", // 指定输出目录
          },
          "references": [ // 指定依赖的工程
            {"path": "./common"}
          ]
        }
  - tsconfig.json // 前后端项目通用基础配置
    {
      "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "strict": true,
        "composite": true, // 增量编译
        "declaration": true
      }
    }
```
- 前端项目构建: `tsc -v src/client`
- 后端项目构建: `tsc -b src/server`
- 输出目录
```
Project
 - dist 
  - client
    - index.js
    - index.d.ts
  - common
    - index.js
    - index.d.ts
  - server
    - index.js
    - index.d.ts
```

### 5. 工程引用配置详解
支持将TypeScript程序的结构分割成更小的组成部分。即一个项目中构建多个单独工程   
1. 在所有工程的基础tsconfig.json配置中添加
```
{
  "compilerOptions": {
    "composite":true,	 开启工程引用和增量编译
    "declaration":true	 生成声明文件
  }
}
```
输出outDir目录由各自工程指定
	
2. 子工程的tsconfig.json
```
{
  "extends":'基础工程路径',
  "compilerOptions": {
  "outDir":'指定该工程及其依赖工程的输出路径'
  },
  "references": [	子工程所依赖的工程
      { "path": "../utils", "prepend": true }	可以在引用中使用prepend选项来启用前置某个依赖的输出
        path:'依赖工程的tsconfig.json的目录或者直接指向到配置文件本身(名字是任意的).'
  ]
}
```
当你引用一个工程时，会发生下面的事：   
导入引用工程中的模块实际加载的是它输出的声明文件（.d.ts）。   
如果引用的工程生成一个outFile，那么这个输出文件的.d.ts文件里的声明对于当前工程是可见的。   
构建模式（后文）会根据需要自动地构建引用的工程。   
3. 构建
```
tsc --build 可简写为b
tsc -b 配置文件地址,如果配置文件名为tsconfig.json,那么文件名则可省略
  tsc -b src test
  
额外指令:
  --verbose：打印详细的日志（可以与其它标记一起使用）
  --dry: 显示将要执行的操作但是并不真正进行这些操作
  --clean: 删除指定工程的输出（可以与--dry一起使用）
  --force: 把所有工程当作非最新版本对待
  --watch: 观察模式（可以与--verbose一起使用）

执行如下操作:
    找到所有引用的工程
    检查它们是否为最新版本
    按顺序构建非最新版本的工程
```


### 6. typescript工程引入js库
如果要使用现有的js文件的话，需要对编译器增加–allowJS参数。可以修改tsconfig.json中的compilerOptions，将其中的"allowJs"设为true，没有该字段的话增加该字段即可。另外在tsconfig中的"include"字段中将js文件的路径添加进去。建议增加一个专门的文件夹，用来存放所有要引入的js文件。比如都放到一个名为"jslibs"的目录中，然后在tsconfig.json的"include"字段中增加配置，匹配"jslibs"下的所有js文件   
![引入js库](/assets/images/allowjs.PNG)   

----

[TypeScript模块(外部模块)、命名空间（内部模块）](https://blog.csdn.net/qq_37708564/article/details/106256723)  
[typescript 工程引用](https://blog.csdn.net/weixin_43294560/article/details/114272582)   
[tsconfig.json](https://www.tslang.cn/docs/handbook/tsconfig-json.html)   