---
title: Typescript常见问题
key: 2021-09-17
tags: typescript
---

### 1. for of与for in 以及forEach的区别
1. for of: 过程可以被打断。循环出的结果是数组中的元素
2. for in: 过程不能被打断，循环出的结果是数组的下标
3. forEach: 过程不能被打断，无法返回
4. every：遍历数组每一项，若全部为true，则返回true
5. some: 遍历数组的每一项，若其中一项为 true，则返回true

```typescript
const list = [4, 5, 6];
list.every((val, index, array) => {
  // val 当前值
  // index 索引值
  // array 数组
  return true; // cotinues
  // return false will quit the interation
})
```

### 2. 型判断typeof,instanceof，以及as断言, 非空断言
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

// 非空断言
// 当你明确知道某个值不可能为 undefined 和 null 时，你可以用 在变量后面加上一个 !（非空断言符号）来告诉编译器："嘿！相信我，我确信这个值不为空！"
function fun(value: string | undefined | null) {
  const str: string = value; // error value 可能为 undefined 和 null
  const str: string = value!; //ok
  const length: number = value.length; // error value 可能为 undefined 和 null
  const length: number = value!.length; //ok
}
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
1. 字面量创建方式：`let reg = /pattern/flags`
2. 构造函数实例创建方式：`let reg = new RegExp(pattern, flags)`

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
- `|` : 或者
- () : 分组
- \n : 匹配换行符
- \b : 匹配边界 字符串的开头和结尾 空格的两边都是边界 => 不占用字符串位数
- ^ : 限定开始位置 => 本身不占位置
- $ : 限定结束位置 => 本身不占位置
- [a-z] : 任意字母 []中的表示任意一个都可以
- [^a-z] : 非字母 []中^代表除了
- [abc] : abc三个字母中的任何一个 [^abc]除了这三个字母中的任何一个字符

次数的量词元字符   
- `*` : 0到多个
- `+` : 1到多个
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

### 9. typescript工程引入js库
如果要使用现有的js文件的话，需要对编译器增加–allowJS参数。可以修改tsconfig.json中的compilerOptions，将其中的"allowJs"设为true，没有该字段的话增加该字段即可。另外在tsconfig中的"include"字段中将js文件的路径添加进去。建议增加一个专门的文件夹，用来存放所有要引入的js文件。比如都放到一个名为"jslibs"的目录中，然后在tsconfig.json的"include"字段中增加配置，匹配"jslibs"下的所有js文件   
![引入js库](/assets/images/allowjs.PNG)   

### 10. 详解TypeScript项目中的tsconfig.json配置
在TS的项目中，TS最终都会被编译JS文件执行，TS编译器在编译TS文件的时候都会先在项目根目录的tsconfig.json文件，根据该文件的配置进行编译，默认情况下，如果该文件没有任何配置，TS编译器会默认编译项目目录下所有的.ts、.tsx、.d.ts文件   

##### 1. 文件选项配置
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

###### 2. compilerOptions
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
##### 3. 工程引用配置
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

##### 4. 工程引用配置详解
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

### 11. 数组排序
```typescript
const numArray: number[] = [6,5,8,2,1];
numArray.sort();

numArray.sort(function(x,y) {
    return x-y
});
```

### 12. @types
@types 是 scoped packages。@types下的所有包会默认被引入，你可以通过修改compilerOptions来修改默认策略。

### 13. 删除文件
同步删除文件： fs.unlinkSync()   
异步删除文件： fs.unlink()

### 14. JSON
JSON数据转字符串: `JSON.stringify(jsondata)`   
字符串转JSON: `JSON.parse(str)`

### 15. 异步函数返回值
```typescript
function getSomething() {
  let r = 0;
  return new Promise(function(resolve){
    setTimeout(function(){
      r = 2;
      resolve(r);
    }, 10)
  })
}

async function compute() {
  let x = await getSomething();
  console.log(x*2)
}
compute()
```

### 16. 计时器
- setTimeout: 在一段指定时间之后执行某个代码任务
- setImmediate: 将在当前事件轮询的末尾处执行
- setInterval: 轮询
```typescript
const timeoutObj = setTimeout(() => {
  console.log('timeout beyond time');
}, 1500);

const immediateObj = setImmediate(() => {
  console.log('immediately executing immediate');
});

const intervalObj = setInterval(() => {
  console.log('interviewing the interval');
}, 500);

clearTimeout(timeoutObj);
clearImmediate(immediateObj);
clearInterval(intervalObj);
```

### 17. 可选链操作符（?.）
`const catName = animal?.cat?.name;`可选链操作符( ?. )允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。?. 操作符的功能类似于 . 链式操作符，不同之处在于，在引用为空(nullish ) (null 或者 undefined) 的情况下不会引起错误，该表达式短路返回值是 undefined。与函数调用一起使用时，如果给定的函数不存在，则返回 undefined。

### 18. 空值合并运算符（??）
空值合并操作符（??）是一个逻辑操作符，当左侧的操作数为 null 或者 undefined 时，返回其右侧操作数，否则返回左侧操作数。   
```typescript
console.log(null ?? 'default string'); // default string
console.log(0 ?? 'default string')     // 0
console.log(1 ?? 'default string')     // 1
console.log(false ?? 'default string') // false
console.log(true ?? 'default string')  // true
console.log('' ?? 'default string')    // ''
console.log('哈哈' ?? 'default string')    // '哈哈'
```

### 19. 双叹号(!!)作用
!!一般用来将后面的表达式强制转换为布尔类型的数据(boolean)，也就是只能是true或者false;
```typescript
const a;  // a为undefined
const b=!!a; // !a = true, !!a = false, b = false
```

### 20. 类型保护机制（is）
TypeScript里有类型保护机制。要定义一个类型保护，我们只要简单地定义一个函数，它的返回值是一个类型谓词：   
- 在使用类型保护时，TS 会进一步缩小变量的类型。例子中，将类型从 any 缩小至了 string
- 类型保护的作用域仅仅在 if 后的块级作用域中生效

```typescript
function isString(test: any): test is string{
    return typeof test === "string";
}

function example(foo: any){
    if(isString(foo)){
        console.log("it is a string" + foo);
        console.log(foo.length); // string function
        // 如下代码编译时会出错，运行时也会出错，因为 foo 是 string 不存在toExponential方法
        console.log(foo.toExponential(2));
    }
    // 编译不会出错，但是运行时出错
    console.log(foo.toExponential(2));
}
example("hello world");
```

### 21. replace all
`str = str.replace(/abc/g, '');`

### 22. 防抖动（debounce）
防止在短时间内过于频繁的执行相同的任务。 当短时间内的频繁是不必要的时候，就可以考虑去抖动，避免资源浪费，或造成不好体验。

### 23. "Record<string, unknown>"
Record<string, unknown> is a utility type that defines an object type whose property keys are of type string and whose property values are of type unknown
extends Record<string, unknown>. It is used to define an object type that can have any number of properties with any key of type string and any value of type unknown.
```typescript
type MyType = {
  foo: string;
} & Record<string, unknown>;

interface MyType extends Record<string, unknown> {
  foo: string;
}

const myObject: MyType = {
  foo: 'bar',
  baz: 42,
};
```
MyType is a type that has a foo property of type string, as well as any additional properties with string keys and unknown values. The myObject variable is an object that conforms to the MyType type.

### 24. function参数默认值，可选参数
```typescript
// 默认值
function test1(a:string, b:string, c:string = 'test1') {
}

// 可选参数
function test2(a:string, b?:string, c:string = 'test2') {
}
```

### 25. 函数调用不加括号
函数调用时加不加括号的区别在于，加括号表示调用函数并返回函数的返回值，不加括号表示返回函数本身
```typescript
function add(x: number, y: number): number {
  return x + y;
}

const result1 = add(1, 2); // result1 的值为 3
const result2 = add; // result2 的类型为 (x: number, y: number) => number
// result1 的值为 3，因为 add(1, 2) 表示调用函数 add 并返回它的返回值。
// 而 result2 的类型为 (x: number, y: number) => number，表示它是一个函数类型，接受两个参数并返回一个数字。
```

### 26. 函数作为对象属性
对象的值可以是任何的数据类型，也可以是个函数   
函数也可以成为对象的属性，如果一个函数作为对象的属性保存，那么，这个函数成为这个对象的方法，调用这个函数就是调用对象的方法（method)
```typescript
const obj = {
  add(x: number, y: number): number {
    return x + y;
  }
};

const result = obj.add(1, 2); // result 的值为 3
```

### 27. bind
bind() 方法可以用于创建一个新函数，该函数与原函数具有相同的函数体，但是 this 值被绑定到指定的对象上。这个方法返回一个新的函数，可以在稍后调用。   
可以使用 bind() 方法来绑定 this 值。例如，假设有一个对象 obj 和一个函数 func，可以使用以下代码来创建一个新函数：   
`let newFunc = func.bind(obj);`   

如果传递了,而且在回调函数中用了this 的话, 就要小心了, 这个this 不一定是指向当前类对象了。   
如果想确保指向的还是那个对象的话, 需要在传递那个方法的时候, 先调用bind(this).   
或者就是在回调的时候, 不要直接func(agrs) 而是改成 func.call(目标对象, args)
```typescript
export class TestCallAndThis {
    /**
     * 不推荐的回调写法
     * 外部调用必须【必须】【必须】在回调参数方法后面添加.bind(this),
     * 否则可能会this异常
     */
    public static callBackTest(arg:number,callBack:Function):void
    {
        //返回 2 x arg
        let result:number=arg*2;
        //不推荐直接调用回调方法，应使用callBack.call(caller,result);
        callBack(result);
    }
    /**
     * 推荐的回调写法
     * @param arg 参数
     * @param caller 调用域 
     * @param method 指定的回调方法（兼容.bind(this) 也可以不加.bind(this) ）
     */
    public static callMethod(arg:number,caller:any,method:Function):void
    {
        //返回 2 x arg
        let result:number=arg*2;
        //推荐的做法 .call(caller,result);
        method.call(caller,result);
      
    }
}

export class Luna {
　//注意观察，this异常的时候的isLoading的值是undefind
  private isLoading:boolean = false;
  private getResult(rst:number):void
  {
      console.log("get rusult:"+rst+this.isLoading);
    
  }
  constructor()
  {
      //不推荐的回调写法， 遗漏了bind（this）
      logic.TestCallAndThis.callBackTest(1,this.getResult);
      //不推荐的回调写法， 使用了bind（this）（ √ ）
      logic.TestCallAndThis.callBackTest(1,this.getResult.bind(this));
    
      //提倡的回调写法 ，有无bind(this)都可以
      logic.TestCallAndThis.callMethod(1,this,this.getResult);
      logic.TestCallAndThis.callMethod(1,this,this.getResult.bind(this));
  }
}
```

### 28. Promise
Promise 是一种用于处理异步操作的对象。Promise 对象表示一个尚未完成、但预计将来会完成的操作，并且可以返回该操作的结果。   
Promise 有三种状态：pending（等待中）、fulfilled（已成功）和 rejected（已失败）。当 Promise 对象处于 pending 状态时，它既不是已成功也不是已失败。当 Promise 对象处于 fulfilled 状态时，它表示操作已成功完成并返回了一个结果。当 Promise 对象处于 rejected 状态时，它表示操作已失败并返回了一个错误。   
可以使用 then() 方法来处理 Promise 对象的结果。
```typescript
promise.then(function(result) {
  console.log(result);
}, function(error) {
  console.log(error);
});
// 这将在 promise 成功时输出 result，在 promise 失败时输出 error。
```

### 29. type 与 interface 的区别
##### 1. 类型别名 type
类型别名用来给一个类型起个新名字，使用 type 创建类型别名，类型别名不仅可以用来表示基本类型，还可以用来表示对象类型、联合类型、元组和交集
```typescript
type userName = string; // 基本类型
type userId = string | number; // 联合类型
type arr = number[]; 

// 对象类型
type Person = {
    id: userId; // 可以使用定义类型
    name: userName;
    age: number;
    gender: string;
    isWebDev: boolean;
};
// 泛型
type Tree<T> = { value: T };

const user: Person = {
    id: "901",
    name: "椿",
    age: 22,
    gender: "女",
    isWebDev: false,
};

const numbers: arr = [1, 8, 9];
```

##### 2. 接口 interface
接口是命名数据结构（例如对象）的另一种方式；与type 不同，interface仅限于描述对象类型。
```typescript
interface Person {
    id: userId;
    name: userName;
    age: number;
    gender: string;
    isWebDev: boolean;
}
```

##### 3. interface和type的相似之处
```typescript
// 都可以描述 Object和Function
type Point = {
  x: number;
  y: number;
};
type SetPoint = (x: number, y: number) => void;

interface Point {
  x: number;
  y: number;
}
interface SetPoint {
  (x: number, y: number): void;
}

// 都可以被继承
// interface 继承 interface
interface Person{
    name:string
}
interface Student extends Person { stuNo: number }
// interface 继承 type
type Person{
    name:string
}
interface Student extends Person { stuNo: number }
// type 继承 type
type Person{
    name:string
}
type Student = Person & { stuNo: number }
// type 继承 interface
interface Person{
    name:string
}
type Student = Person & { stuNo: number }

// 实现 implements
interface ICat{
    setName(name:string): void;
}
class Cat implements ICat{
    setName(name:string):void{
        // todo
    }
}

// type 
type ICat = {
    setName(name:string): void;
}
class Cat implements ICat{
    setName(name:string):void{
        // todo
    }
}
```

##### 4. 区别
```typescript
// type可以定义基本类型别名, 但是interface无法定义
type userName = string
type stuNo = number

// type可以声明联合类型
type Student = {stuNo: number} | {classId: number}

// type可以声明 元组类型
type Data = [number, string];

// type不支持声明合并
// 如果你多次声明一个同名的接口，TypeScript 会将它们合并到一个声明中，并将它们视为一个接口。这称为声明合并
interface Person { name: string }
interface Person { age: number }
let user: Person = {
    name: "Tolu",
    age: 0,
};

type Person { name: string }; 
// Error: 标识符“Person”重复。ts(2300)
type Person { age: number }
```

##### 5. 使用场景
对于 React 组件中 props及 state，使用type，这样能够保证使用组件的地方不能随意在上面添加属性。如果有自定义需求，可通过 HOC二次封装。   
编写三方库时使用interface，其更加灵活自动的类型合并可应对未知的复杂使用场景。   


----

[TypeScript小状况之遍历对象属性](https://juejin.cn/post/6844904147146260488)   
[node.js中fs模块+path模块](https://blog.csdn.net/WJ930708/article/details/81738256)  
[Nodejs正则表达式概述](https://www.jianshu.com/p/10232b4afedc)  
[Nodejs正则表达式](https://blog.csdn.net/dc_show/article/details/42083377)  
[正则表达式在线测试](https://c.runoob.com/front-end/854/)  
[TypeScript模块(外部模块)、命名空间（内部模块）](https://blog.csdn.net/qq_37708564/article/details/106256723)  
[typescript 工程引用](https://blog.csdn.net/weixin_43294560/article/details/114272582)   
[tsconfig.json](https://www.tslang.cn/docs/handbook/tsconfig-json.html)   
[拿JS异步函数返回值的几种方式](https://blog.csdn.net/weixin_40970987/article/details/82255252)    
[Node.js 中的定时器](https://nodejs.org/zh-cn/docs/guides/timers-in-node/)   
[TypeScript: this bind 和 回调的正确用法](https://www.cnblogs.com/naiking/p/9836289.html)   
[type 与 interface 的区别](https://juejin.cn/post/7072945053936648200)   