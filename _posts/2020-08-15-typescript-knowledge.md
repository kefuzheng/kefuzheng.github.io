---
title: Typescript基础语法
key: 2020-08-10
tags: typescript
---

### 1. 预备知识
TypeScript起初是微软开发的以JavaScript为基础的编程语言，他兼容JavaScript的所有特性，并扩展了JS的类型系统，使得用户在大型系统开发中更加游刃有余，VS Code天然支持TypeScript，帮助开发者写出更加稳定、安全的代码。因此所有文档的示例，包括插件本身，绝大部分都是使用TypeScript开发的，俗话说“不积跬步无以至千里”，当你足够了插件的基础之后，阅读文档才会更加顺利。  
VS Code已经禁用了CSS和HTML
### 2. 基础语法
`tsc Test.ts`   
单行注释 ( // ) − 在 // 后面的文字都是注释内容。   
多行注释 (/* */) − 这种注释可以跨越多行。   
每行指令都是一段语句，你可以使用分号或不使用， 分号在 TypeScript 中是可选的，建议使用。   
TypeScript 会忽略程序中出现的空格、制表符和换行符。空格、制表符通常用来缩进代码，使代码易于阅读和理解。  
### 3. 基础类型
##### 1. 任意类型any和unknow
声明为 any 的变量可以赋予任意类型的值。任意值是 TypeScript 针对编程时类型不明确的变量使用的一种数据类型。   
unknow可以保证类型的安全
```typescript
let x: any = 1;    // 数字类型
x = 'I am who I am';    // 字符串类型
x = false;    // 布尔类型
```
##### 2. 数字类型number
双精度 64 位浮点值。它可以用来表示整数和分数
```typescript
let binaryLiteral: number = 0b1010; // 二进制
let octalLiteral: number = 0o744;    // 八进制
let decLiteral: number = 6;    // 十进制
let hexLiteral: number = 0xf00d;    // 十六进制
```
##### 3. 字符串类型string
一个字符系列，使用单引号（'）或双引号（"）来表示字符串类型。反引号（`）来定义多行文本和内嵌表达式。
```typescript
let name: string = "typescript";
let years: number = 5;
let words: string = `您好，今年是 ${ name } 发布 ${ years + 1} 周年`;
```
##### 4. 布尔类型boolean
表示逻辑值：true 和 false。
```typescript
let flag: boolean = true;
```
##### 5. 数组类型
声明变量为数组。
```typescript
// 在元素类型后面加上[]
let arr: number[] = [1, 2];

// 或者使用数组泛型
let arr: Array<number> = [1, 2];
```
##### 6. 元组tuple
元组类型是一种特殊的数组，是已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。元组中允许存储不同类型的元素，元组可以作为参数传递给函数。
```typescript
let x: [string, number];
x = ['Runoob', 1];    // 运行正常
x = [1, 'Runoob'];    // 报错
console.log(x[0]);    // 输出 Runoob
```
##### 7. 联合Union类型
联合类型（Union Types）可以通过管道(|)将变量设置多种类型，赋值时可以根据设置的类型来赋值。只能赋值指定的类型，如果赋值其它类型就会报错。
```typescript
let union : string | number // 既可以是string类型，也可以是number

let val:string|number 
val = 12 
console.log("数字为 "+ val) 
val = "Runoob" 
console.log("字符串为 " + val)

// 联合类型数组
let arr:number[]|string[]; 
arr = [1,2,4] 
arr = ["Runoob","Google","Taobao"] 
```
##### 8. 字面量类型literal
```typescript
let union1 : 0 | 1 | 2 // 是number类型，取值范围0，1，2
union1 = 0 
union1 = 3  // 报错

let literal : 1 | "2" | true | [1,2,3,4]
```
##### 9. 枚举enum
枚举类型用于定义数值集合。
```typescript
enum Color {Red, Green, Blue};
let c: Color = Color.Blue;
console.log(c);    // 输出 2

enum Color1 {
    Red = 1, 
    Green = 2, 
    Blue = "blue"
    };
```
##### 10. void
用于标识方法返回值的类型，表示该方法没有返回值。
```typescript
function hello(): void {
    alert("Hello Runoob");
}
```
##### 11. null和undefined
null表示对象值缺失。undefined用于初始化变量为一个未定义的值
用 typeof 检测 null 返回是 object。typeof 一个没有值的变量会返回 undefined。   
Null 和 Undefined 是其他任何类型（包括 void）的子类型，可以赋值给其它类型，如数字类型，此时，赋值后的类型会变成 null 或 undefined。而在TypeScript中启用严格的空校验（--strictNullChecks）特性，就可以使得null 和 undefined 只能被赋值给 void 或本身对应的类型
```typescript
// 启用 --strictNullChecks
let x: number;
x = 1; // 运行正确
x = undefined;    // 运行错误
x = null;    // 运行错误
```
##### 12. never
never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。这意味着声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环）
```typescript
let x: never;
let y: number;

// 运行错误，数字类型不能转为 never 类型
x = 123;

// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();

// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();

// 返回值为 never 的函数可以是抛出异常的情况
function error(message: string): never {
    throw new Error(message);
}

// 返回值为 never 的函数可以是无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}
```
##### 13. 类型适配（类型断言）
```typescript
let msg : any;
msg = "abc";
msg.endWith("c");
let msg1 = (<string>msg).endWith("c");
let msg2 = (msg as string).endWith("c");
```

### 4. 变量声明
`var [变量名] : [类型] = 值;` 声明变量的类型，但没有初始值，变量值会设置为 undefined;声明变量没有设置类型和初始值，类型可以是任意类型，默认初始值为 undefined：
##### 1. 类型断言（Type Assertion）
类型断言可以用来手动指定一个值的类型，即允许变量从一种类型更改为另一种类型。   
`<类型>值 或 值 as 类型`
```typescript
var str = '1' 
var str2:number = <number> <any> str   //str、str2 是 string 类型
console.log(str2)
```
##### 2. 变量作用域
- 全局作用域 − 全局变量定义在程序结构的外部，它可以在你代码的任何位置使用。
- 类作用域 − 这个变量也可以称为 字段。类变量声明在一个类里头，但在类的方法外面。 该变量可以通过类的对象来访问。类变量也可以是静态的，静态的变量可以通过类名直接访问。
- 局部作用域 − 局部变量，局部变量只能在声明它的一个代码块（如：方法）中使用。
```typescript
var global_num = 12          // 全局变量
class Numbers { 
   num_val = 13;             // 实例变量
   static sval = 10;         // 静态变量
   
   storeNum():void { 
      var local_num = 14;    // 局部变量
   } 
} 
console.log("全局变量为: "+global_num)  
console.log(Numbers.sval)   // 静态变量
var obj = new Numbers(); 
console.log("实例变量: "+obj.num_val)
```
##### 3. let和var的区别
let和var的区别体现在作用域上。var的作用域被规定为一个函数作用域，而let则被规定为块作用域，块作用域要比函数作用域小一些，但是如果两者既没在函数中，也没在块作用域中定义，那么两者都属于全局作用域。   
不推荐使用var   
```typescript
function  aFun1(){
    // i 对于for循环外的范围是不可见的(i is not defined)
    for(let i = 1; i<5; i++){
        //  i只有在这里是可见的
    }
    // i 对于for循环外的范围是不可见的(i is not defined)
}
function aFun2(){
    // i 对于for循环外的范围是可见的
    for(var i = 1;i<5; i++){
        // i 在for 在整个函数体内都是可见的
    }
    // i 对于for循环外的范围是可见的
}
```
var允许在同一作用域中声明同名的变量，而let不可以
```typescript
let me  = 'foo';
let me  = 'bar'; //SyntaxError: Identifier 'me' has already been declared

var me = 'foo';
var me = 'bar'; //这里me被替代了，是可以重复声明的
```
##### 4. const
定义常量

### 5. 运算符
##### 1. 逻辑运算符
- &&, and, (x < 10 && y > 1) 为 true
- ||, or, (x==5 || y==5) 为 false
- !, not, !(x==y) 为 true

##### 2. 三元运算符 (?)
`Test ? expr1 : expr2`
##### 3. 类型运算符
typeof 运算符   
```typescript
var num = 12 
console.log(typeof num);   //输出结果: number
```
instanceof, instanceof 运算符用于判断对象是否为指定的类型
### 6. 条件语句
- if 语句 - 只有当指定条件为 true 时，使用该语句来执行代码
- if...else 语句 - 当条件为 true 时执行代码，当条件为 false 时执行其他代码
- if...else if....else 语句- 使用该语句来选择多个代码块之一来执行
- switch 语句 - 使用该语句来选择多个代码块之一来执行
```typescript
var num:number = 12; 
if (num % 2==0) { 
    console.log("偶数"); 
} else {
    console.log("奇数"); 
}

var grade:string = "A"; 
switch(grade) { 
    case "A": { 
        console.log("优"); 
        break; 
    } 
    default: { 
        console.log("非法输入"); 
        break;              
    } 
}
```

### 7. 循环
```typescript
var num:number = 5; 
var i:number; 
var factorial = 1; 
 
for(i = num;i>=1;i--) {
   factorial *= i;
}
console.log(factorial)

//for...in 循环
var j:any; 
var n:any = "a b c" 
for(j in n) {
    console.log(n[j])  
}

//for...of 循环
let someArray = [1, "string", false];
 
for (let entry of someArray) {
    console.log(entry); // 1, "string", false
}

// forEach 循环
let list = [4, 5, 6];
list.forEach((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
});

every 循环
let list = [4, 5, 6];
list.every((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
    return true; // Continues
    // Return false will quit the iteration
});

无限循环
for(;;) { 
   console.log("这段代码会不停的执行") 
}
while(true) { 
   console.log("这段代码会不停的执行") 
}
```
### 8. 函数
##### 1. 函数定义
```typescript
function greet():string { // 返回一个字符串
    return "Hello World" 
} 
 
function caller() { 
    var msg = greet() // 调用 greet() 函数 
    console.log(msg) 
} 
 
// 调用函数
caller()
```
##### 2. 可选参数
```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
 
let result1 = buildName("Bob");  // 正确
let result2 = buildName("Bob", "Adams", "Sr.");  // 错误，参数太多了
let result3 = buildName("Bob", "Adams");  // 正确
```
##### 3. 默认参数
```typescript
function calculate_discount(price:number,rate:number = 0.50) { 
    var discount = price * rate; 
    console.log("计算结果: ",discount); 
} 
calculate_discount(1000) 
calculate_discount(1000,0.30)
```
##### 4. 剩余参数
```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}
  
let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```
##### 5. 匿名函数
```typescript
var res = function(a:number,b:number) { 
    return a*b;  
}; 
console.log(res(12,2))

// 匿名函数自调用
(function () { 
    var x = "Hello!!";   
    console.log(x)     
})()
```
##### 6. 构造函数
```typescript
var myFunction = new Function("a", "b", "return a * b"); 
var x = myFunction(4, 3); 
console.log(x);
```
##### 7. Lambda 函数
```typescript
var foo = (x:number)=>10 + x 
console.log(foo(100))      //输出结果为 110

var foo = (x:number)=> {    
    x = 10 + x 
    console.log(x)  
} 
foo(100)

// 不指定函数的参数类型，通过函数内来推断参数类型:
var func = (x)=> { 
    if(typeof x=="number") { 
        console.log(x+" 是一个数字") 
    } else if(typeof x=="string") { 
        console.log(x+" 是一个字符串") 
    }  
} 
func(12) 
func("Tom")

// 单个参数 () 是可选的：
var display = x => { 
    console.log("输出为 "+x) 
} 
display(12)

// 无参数时可以设置空括号：
var disp =()=> { 
    console.log("Function invoked"); 
} 
disp();
```
### 9. Number
Number 对象是原始数值的包装对象。   
`var num = new Number(value);`    
##### 1. toFixed()
把数字转换为字符串，并对小数点指定位数。
```typescript
var num3 = 177.234 
console.log("num3.toFixed() 为 "+num3.toFixed())    // 输出：177
console.log("num3.toFixed(2) 为 "+num3.toFixed(2))  // 输出：177.23
console.log("num3.toFixed(6) 为 "+num3.toFixed(6))  // 输出：177.234000
```
##### 2. toPrecision()
把数字格式化为指定的长度。
```typescript
var num = new Number(7.123456); 
console.log(num.toPrecision());  // 输出：7.123456 
console.log(num.toPrecision(1)); // 输出：7
console.log(num.toPrecision(2)); // 输出：7.1
```
##### 3. toString()
把数字转换为字符串，使用指定的基数。数字的基数是 2 ~ 36 之间的整数。若省略该参数，则使用基数 10。
```typescript
var num = new Number(10); 
console.log(num.toString());  // 输出10进制：10
console.log(num.toString(2)); // 输出2进制：1010
console.log(num.toString(8)); // 输出8进制：12
```
##### 4. valueOf()
返回一个 Number 对象的原始数字值。
```typescript
var num = new Number(10); 
console.log(num.valueOf()); // 输出：10
```
### 10. String（字符串）
String 对象用于处理文本（字符串）。
```typescript
var txt = new String("string");
或者更简单方式：
var txt = "string";
```
charAt(), indexOf(), lastIndexOf(), match(), replace(), slice(), split(), substring()
### 11. Array(数组)
```typescript
var nums:number[] = [1,2,3,4] 
console.log(nums[0]); 
console.log(nums[1]); 
console.log(nums[2]); 
console.log(nums[3]);

// Array 对象
var arr_names:number[] = new Array(4)  
for(var i = 0; i<arr_names.length; i++) { 
        arr_names[i] = i * 2 
        console.log(arr_names[i]) 
}

var sites:string[] = new Array("Google","Runoob","Taobao","Facebook") 
```
##### 1. 数组解构
可以把数组元素赋值给变量
```typescript
var arr:number[] = [12,13] 
var[x,y] = arr // 将数组的两个元素赋值给变量 x 和 y
console.log(x) 
console.log(y)
```
### 12. Map 对象
```typescript
let myMap = new Map([
        ["key1", "value1"],
        ["key2", "value2"]
    ]); 

let nameSiteMapping = new Map();
 
// 设置 Map 对象
nameSiteMapping.set("Google", 1);
nameSiteMapping.set("Runoob", 2);
nameSiteMapping.set("Taobao", 3);
 
// 获取键对应的值
console.log(nameSiteMapping.get("Runoob"));     //40
 
// 判断 Map 中是否包含键对应的值
console.log(nameSiteMapping.has("Taobao"));       //true
console.log(nameSiteMapping.has("Zhihu"));        //false
 
// 返回 Map 对象键/值对的数量
console.log(nameSiteMapping.size);                //3
 
// 删除 Runoob
console.log(nameSiteMapping.delete("Runoob"));    // true
console.log(nameSiteMapping);
// 移除 Map 对象的所有键/值对
nameSiteMapping.clear();             //清除 Map
console.log(nameSiteMapping);
```
##### 1. 迭代 Map
```typescript
// 迭代 Map 中的 key
for (let key of nameSiteMapping.keys()) {
    console.log(key);                  
}
 
// 迭代 Map 中的 value
for (let value of nameSiteMapping.values()) {
    console.log(value);                 
}
 
// 迭代 Map 中的 key => value
for (let entry of nameSiteMapping.entries()) {
    console.log(entry[0], entry[1]);   
}
 
// 使用对象解析
for (let [key, value] of nameSiteMapping) {
    console.log(key, value);            
}
```
### 14. 接口
接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。
```typescript
interface IPerson { 
    firstName:string, 
    lastName:string, 
    sayHi: ()=>string 
} 
 
var customer:IPerson = { 
    firstName:"Tom",
    lastName:"Hanks", 
    sayHi: ():string =>{return "Hi there"} 
} 
 
console.log("Customer 对象 ") 
console.log(customer.firstName) 
console.log(customer.lastName) 
console.log(customer.sayHi()) 
```
##### 1. 联合类型和接口
```typescript
interface RunOptions { 
    program:string; 
    commandline:string[]|string|(()=>string); 
} 
 
// commandline 是字符串
var options:RunOptions = {program:"test1",commandline:"Hello"}; 
console.log(options.commandline)  
 
// commandline 是字符串数组
options = {program:"test1",commandline:["Hello","World"]}; 
console.log(options.commandline[0]); 
console.log(options.commandline[1]);  
 
// commandline 是一个函数表达式
options = {program:"test1",commandline:()=>{return "**Hello World**";}}; 
 
var fn:any = options.commandline; 
console.log(fn());
```
##### 2. 接口和数组
接口中我们可以将数组的索引值和元素设置为不同类型，索引值可以是数字或字符串。
```typescript
interface namelist { 
   [index:number]:string 
} 
 
var list2:namelist = ["John",1,"Bran"] // 错误元素 1 不是 string 类型
interface ages { 
   [index:string]:number
} 
 
var agelist:ages; 
agelist["John"] = 15   // 正确 
agelist[2] = "nine"   // 错误
```
##### 3. 接口继承
允许接口继承多个接口, 继承使用关键字 extends。
```typescript
// 单继承实例
interface Person { 
   age:number 
} 
 
interface Musician extends Person { 
   instrument:string 
} 
 
var drummer = <Musician>{}; 
drummer.age = 27 
drummer.instrument = "Drums" 
console.log("年龄:  "+drummer.age)
console.log("喜欢的乐器:  "+drummer.instrument)

// 多继承实例
interface IParent1 { 
    v1:number 
} 
 
interface IParent2 { 
    v2:number 
} 
 
interface Child extends IParent1, IParent2 { } 
var Iobj:Child = { v1:12, v2:23} 
console.log("value 1: "+Iobj.v1+" value 2: "+Iobj.v2)
```
### 16. 类
```typescript
class Car { 
    // 字段 
    engine:string; 
 
    // 构造函数 
    constructor(engine:string) { 
        this.engine = engine 
    }  
 
    // 方法 
    disp():void { 
        console.log("发动机为 :   "+this.engine) 
    } 
}

// 创建一个对象
var obj = new Car("XXSY1")
 
// 访问字段
console.log("读取发动机型号 :  "+obj.engine)  
 
// 访问方法
obj.disp()
```
##### 1. 类的继承
单继承
```typescript
class PrinterClass { 
   doPrint():void {
      console.log("父类的 doPrint() 方法。") 
   } 
} 
 
class StringPrinter extends PrinterClass { 
   doPrint():void { 
      super.doPrint() // 调用父类的函数
      console.log("子类的 doPrint()方法。")
   } 
}
```
##### 2. static 关键字
static 关键字用于定义类的数据成员（属性和方法）为静态的，静态成员可以直接通过类名调用。
```typescript
class StaticMem {  
   static num:number; 
   
   static disp():void { 
      console.log("num 值为 "+ StaticMem.num) 
   } 
} 
 
StaticMem.num = 12     // 初始化静态变量
StaticMem.disp()       // 调用静态方法
```
##### 3. instanceof 运算符
instanceof 运算符用于判断对象是否是指定的类型，如果是返回 true，否则返回 false。
```typescript
class Person{ } 
var obj = new Person() 
var isPerson = obj instanceof Person; 
console.log("obj 对象是 Person 类实例化来的吗？ " + isPerson);
```
##### 4. 访问控制修饰符
- public（默认） : 公有，可以在任何地方被访问。
- protected : 受保护，可以被其自身以及其子类和父类访问。
- private : 私有，只能被其定义所在的类访问。

```typescript
class Encapsulate { 
   str1:string = "hello" 
   private str2:string = "world" 
}
 
var obj = new Encapsulate() 
console.log(obj.str1)     // 可访问 
console.log(obj.str2)   // 编译错误， str2 是私有的
```
##### 5. 类和接口
类可以实现接口，使用关键字 implements，并将 interest 字段作为类的属性使用。   
构造函数不能重载    
在构造函数加访问修饰符public，protected，private，会自动完成变量的声明以及赋值，但是参数不能是可选参数
```typescript
interface ILoan { 
   interest:number 
   drawPoint: () => void;
} 
 
class AgriLoan implements ILoan { 
   drawPoint: () =>{
       console.log("drawPoint")
   };
   
   constructor(interest:number,rebate:number) { 
      this.interest = interest 
      this.rebate = rebate 
   } 
} 

class BlikLoan implements ILoan { 
   drawPoint: () =>{
       console.log("drawPoint")
   };
   // 在构造函数加访问修饰符public，protected，private，会自动完成变量的声明以及赋值，但是参数不能是可选参数
   constructor(public interest:number, public rebate:number) { 
   } 
} 
 
var obj = new AgriLoan(10,1) 
console.log("利润为 : "+obj.interest+"，抽成为 : "+obj.rebate )
```
### 17. 对象Object
对象是包含一组键值对的实例。 值可以是标量、函数、数组、对象等，如下实例：
```typescript
var object_name = { 
    key1: "value1", // 标量
    key2: "value",  
    key3: function() {
        // 函数
    }, 
    key4:["content1", "content2"] //集合
}
```
##### 1. 类型模板
```typescript
var sites = {
    site1: "Runoob",
    site2: "Google",
    sayHello: function () { } // 类型模板
};
sites.sayHello = function () {
    console.log("hello " + sites.site1);
};
sites.sayHello();

// 对象也可以作为一个参数传递给函数
var sites = { 
    site1:"Runoob", 
    site2:"Google",
}; 
var invokesites = function(obj: { site1:string, site2 :string }) { 
    console.log("site1 :"+obj.site1) 
    console.log("site2 :"+obj.site2) 
} 
invokesites(sites)
```
##### 2. 鸭子类型(Duck Typing)
鸭子类型（英语：duck typing）是动态类型的一种风格，是多态(polymorphism)的一种形式。
```typescript
interface IPoint { 
    x:number 
    y:number 
} 
function addPoints(p1:IPoint,p2:IPoint):IPoint { 
    var x = p1.x + p2.x 
    var y = p1.y + p2.y 
    return {x:x,y:y} 
} 
 
// 正确
var newPoint = addPoints({x:3,y:4},{x:5,y:1})  
 
// 错误 
var newPoint2 = addPoints({x:1},{x:4,y:3})
```
##### 3. 泛型
```typescript
let lastInArray = <T>(arr: T[]) =>{
    return arr[arr.length - 1];
}
const l1 = lastInArray([1,2,3,4]);
const l2 = lastInArray<string>(["a","b","c","d"]);
const l2 = lastInArray<string | number>(["a","b","c","d", 3, 4]);
```

### 18. 命名空间
```typescript
namespace SomeNameSpaceName { 
   export interface ISomeInterfaceName {      }  
   export class SomeClassName {      }  
}
```
以上定义了一个命名空间 SomeNameSpaceName，如果我们需要在外部可以调用 SomeNameSpaceName 中的类和接口，则需要在类和接口添加 export 关键字,要在另外一个命名空间调用语法格式为：   
`SomeNameSpaceName.SomeClassName;`   
如果一个命名空间在一个单独的 TypeScript 文件中，则应使用三斜杠 /// 引用它，语法格式如下：   
`/// <reference path = "SomeFileName.ts" />`   
命名空间支持嵌套，即你可以将命名空间定义在另外一个命名空间里头。
```typescript
namespace namespace_name1 { 
    export namespace namespace_name2 {
        export class class_name {    } 
    } 
}
```
### 19. 模块
模块是在其自身的作用域里执行，并不是在全局作用域，这意味着定义在模块里面的变量、函数和类等在模块外部是不可见的，除非明确地使用 export 导出它们。类似地，我们必须通过 import 导入其他模块导出的变量、函数、类等。   
两个模块之间的关系是通过在文件级别上使用 import 和 export 建立的。   
模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。   
```typescript
// 模块导出使用关键字 export 关键字
// 文件名 : SomeInterface.ts 
export interface SomeInterface { 
   // 代码部分
}

//在另外一个文件使用该模块就需要使用 import 关键字来导入
import someInterfaceRef = require("./SomeInterface");
```
### 20. 声明文件
TypeScript 中，我们并不知道 $ 或 jQuery 是什么东西,需要使用 declare 关键字来定义它的类型，帮助 TypeScript 判断我们传入的参数类型对不对：
```typescript
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```
declare 定义的类型只会用于编译时的检查，编译结果中会被删除。   
##### 1. 声明文件
声明文件以 .d.ts 为后缀, runoob.d.ts。   
TypeScript 引入声明文件语法格式：
`/// <reference path = " runoob.d.ts" />`   
声明文件或模块的语法格式如下：   
`declare module Module_Name {}`