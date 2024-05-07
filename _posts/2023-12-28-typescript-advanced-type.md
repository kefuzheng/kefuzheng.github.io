---
title: TypeScript 高级类型和内置类型
key: 2023-12-28
tags: typescript
---

### 1. 泛型
泛型允许程序员在强类型程序设计语言中编写代码时使用一些以后才指定的类型，在实例化时作为参数指明这些类型。   
泛型通过一对尖括号来表示(<>)，尖括号内的字符被称为类型变量，这个变量用来表示类型。
```ts
function copy<T>(arg: T): T {
  if (typeof arg === 'object') {
    return JSON.parse(
      JSON.stringify(arg)
    )
  } else {
    return arg
  }
}

// 这个类型 T，在没有调用 copy 函数的时候并不确定，只有调用 copy 的时候，我们才知道 T 具体代表什么类型。
const str = copy<string>('my name is typescript')
```

### 2. 高级类型
##### 1. 交叉类型（&）
交叉类型说简单点就是将多个类型合并成一个类型，其语法规则和逻辑 “与” 的符号一致。
```ts
T & U

// example
interface Button {
  type: string
  text: string
}

interface Link {
  alt: string
  href: string
}

const linkBtn: Button & Link = {
  type: 'danger',
  text: '跳转到百度',
  alt: '跳转到百度',
  href: 'http://www.baidu.com'
}
```

##### 2. 联合类型（|）
联合类型的语法规则和逻辑 “或” 的符号一致，表示其类型为连接的多个类型中的任意一个。
```ts
T | U

interface Button {
  type: 'default' | 'primary' | 'danger'
  text: string
}

const btn: Button = {
  type: 'primary',
  text: '按钮'
}

```

##### 3. 类型别名（type）
前面提到的交叉类型与联合类型如果有多个地方需要使用，就需要通过类型别名的方式，给这两种类型声明一个别名
```ts
type Alias = T | U

type InnerType = 'default' | 'primary' | 'danger'

interface Button {
  type: InnerType
  text: string
}

interface Alert {
  type: ButtonType
  text: string
}
```

##### 4. 类型索引（keyof）
keyof 类似于 Object.keys ，用于获取一个接口中 Key 的联合类型。
```ts
interface Button {
    type: string
    text: string
}

type ButtonKeys = keyof Button
// 等效于
type ButtonKeys = "type" | "text"
```

##### 5. 类型约束（extends）
这里的 extends 关键词不同于在 class 后使用 extends 的继承作用，泛型内使用的主要作用是对泛型加以约束
```ts
type BaseType = string | number | boolean

// 这里表示 copy 的参数
// 只能是字符串、数字、布尔这几种基础类型
function copy<T extends BaseType>(arg: T): T {
  return arg
}

// extends 经常与 keyof 一起使用，例如我们有一个方法专门用来获取对象的值，但是这个对象并不确定，我们就可以使用 extends 和 keyof 进行约束。
// 这里的 getValue 方法就能根据传入的参数 obj 来约束 key 的值。
function getValue<T, K extends keyof T>(obj: T, key: K) {
  return obj[key]
}

const obj = { a: 1 }
const a = getValue(obj, 'a')
```

##### 6. 类型映射（in）
in 关键词的作用主要是做类型的映射，遍历已有接口的 key 或者是遍历联合类型
```ts
type Readonly<T> = {
    // 然后 P in 相当于执行了一次 forEach 的逻辑
    readonly [P in keyof T]: T[P];
};

interface Obj {
  a: string
  b: string
}

type ReadOnlyObj = Readonly<Obj>
// 相当于
type ReadOnlyObj = {
    readonly a: Obj['a'];
    readonly b: Obj['b'];
}

interface ReadOnlyObj {
    readonly a: string;
    readonly b: string;
}
```

##### 7. 条件类型（U ? X : Y）
条件类型的语法规则和三元表达式一致，如果 T 是 U 的子集，就是类型 X，否则为类型 Y   
`T extends U ? X : Y`

### 3. 内置类型
##### 1. Partial（部分的）
让传入类型中的所有属性变成都是可选的
```ts
/**
 * Make all properties in T optional
 */
type Partial<T> = {
    [P in keyof T]?: T[P];
};

export interface Student {
  name: string;
  age: number;
}

const student1: Student = {} // 错误，student1的类型是Student，Student默认所有的属性都是不能为空的

const student2: Partial<Student> = {} // 正确
```

##### 2. Required（必须的）
跟Partial的作用是相反的，是让传入类型中的所有属性变成都是必填的
```ts
/**
 * Make all properties in T required
 */
type Required<T> = {
    [P in keyof T]-?: T[P];
};

export interface Student {
  name?: string;
  age?: number;
}

const student1: Student = {} // 正确， 变量student1的类型是Student，Student默认所有的属性都是可以为空的，所有不会报错

const student2: Required<Student> = {} // 错误
```

##### 3. Readonly（只读的）
让传入类型中的所有属性变成都是只读的（不能修改属性）
```ts
/**
 * Make all properties in T readonly
 */
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

export interface Student {
  name: string;
  age: number;
}

const student1: Student = {
  name: '张三',
  age: 20
}
student1.age = 21 // 正确， student1的属性age重新赋值不会报错

const student2: Readonly<Student> = {
  name: '李四',
  age: 20
}
student2.age = 21 // 错误， student2的属性age重新赋值就会报错，因为student2所有的属性都是只读的
```

##### 4. Pick（选择）
选择传入类型中的部分属性组成新类型
```ts
/**
 * From T, pick a set of properties whose keys are in the union K
 */
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};

export interface Student {
  name: string;
  age: number;
}

const student1: Student = {  // 正确，student1可以有所有属性name和age
  name: '张三',
  age: 20
}

const student2: Pick<Student, 'name'> = {  // 正确，变量student2就只能有属性name
  name: '李四'
}

const student3: Pick<Student, 'name'> = {  // 错误，变量student3加上属性age就会报错
  name: '王五',
  age: 20
}
```

##### 5. Record（记录）
构建一个类型，这个类型用来描述一个对象，这个对象的属性都具有相同的类型
```ts
/**
 * Construct a type with a set of properties K of type T
 */
type Record<K extends keyof any, T> = {
    [P in K]: T;
};

// Record应该是日常使用频率较高的内置类型了，主要用来描述对象，一般建议是不用Object来描述对象，而是用Record代替，Record<string, any>几乎可以说是万金油了
export const student1: Record<string, any> = {
  name: '张三',
  age: 20
}
```

##### 6. Exclude（排除）
针对联合类型，排除相同的，留下不同的
```ts
/**
 * Exclude from T those types that are assignable to U
 */
type Exclude<T, U> = T extends U ? never : T;

export type PersonAttr = 'name' | 'age'

export type StudentAttr = 'name' | 'age' | 'class' | 'school'

const student1: Exclude<StudentAttr, PersonAttr>  // student1就只能被赋值为'class' 或者'school'
```

##### 7. Extract（取出）
与Exclude相反，针对联合类型，排除不同的的，取出相同的
```ts
/**
 * Extract from T those types that are assignable to U
 */
type Extract<T, U> = T extends U ? T : never;

export type PersonAttr = 'name' | 'age'

export type StudentAttr = 'name' | 'age' | 'class' | 'school'

const student1: Extract<StudentAttr, PersonAttr>  // student1就只能被赋值为'name'或者'age'
```

##### 8. Omit（省略）
传入一个类型，和这个类型的几个属性，把传入的属性省略掉，组成一个新类型
```ts
/**
 * Construct a type with the properties of T except for those in type K.
 */
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;

export interface Student {
  name: string;
  age: number;
  class: string;
  school: string;
}

export type PersonAttr = 'name' | 'age'

export type StudentAttr = 'name' | 'age' | 'class' | 'school'

const student1: Omit<Student, PersonAttr> = {} // student1报错，提示没有属性'class'、'school'
```

##### 9. NonNullable（不能为null）
不能为空
```ts
/**
 * Exclude null and undefined from T
 */
type NonNullable<T> = T extends null | undefined ? never : T;

export interface Student {
  name: string;
  age: number;
}

const student1: NonNullable<Student | undefined | null> = null // student1赋值为null会报错
```

##### 10. Parameters（参数）
获取传入函数的参数组成的类型
```ts
/**
 * Obtain the parameters of a function type in a tuple
 */
type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;

export interface Student {
  name: string;
  age: number;
}

export interface StudentFunc {
  (name: string, age: number): Student
}

const student1: Parameters<StudentFunc>  // student1的类型为[name: string, age: number]
```

##### 11. ConstructorParameters（构造参数）
获取传入构造函数的参数组成的类型
```ts
/**
 * Obtain the parameters of a constructor function type in a tuple
 */
type ConstructorParameters<T extends abstract new (...args: any) => any> = T extends abstract new (...args: infer P) => any ? P : never;

export interface Student {
  name: string;
  age: number;
}

export interface StudentConstructor {
  new (name: string, age: number): Student
}

const student1: ConstructorParameters<StudentConstructor>  // student1的类型为[name: string, age: number]
```

##### 12. ReturnType（返回类型）
获取传入函数的返回类型
```ts
/**
 * Obtain the return type of a function type
 */
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;

export interface Student {
  name: string;
  age: number;
}

export interface StudentFunc {
  (name: string, age: number): Student
}

const student1: ReturnType<StudentFunc> = {}  // student1的类型为Student
```

##### 13. InstanceType（构造返回类型、实例类型）
```ts
获取传入构造函数的返回类型

/**
 * Obtain the return type of a constructor function type
 */
type InstanceType<T extends abstract new (...args: any) => any> = T extends abstract new (...args: any) => infer R ? R : any;

const Student = class {
  name: string;
  age: number;
  constructor (name: string, age: number) {
    this.name = name
    this.age = age
  }
  showInfo () {
    console.log('name: ', this.name, 'age: ', this.age);
  }
}

const student1: InstanceType<typeof Student> = new Student('张三', 20)
```

##### 14. Uppercase（大写）
```ts
/**
 * Convert string literal type to uppercase
 */
type Uppercase<S extends string> = intrinsic;

export type StudentSexType = 'male' | 'female'

const studentSex: Uppercase<StudentSexType> = 'MALE'
```

##### 15. Lowercase（小写）
```ts
/**
 * Convert string literal type to lowercase
 */
type Lowercase<S extends string> = intrinsic;

export type StudentSexType = 'MALE' | 'FEMALE'

const studentSex: Lowercase<StudentSexType> = 'male'
```

##### 16. Capitalize（首字母大写）
```ts
/**
 * Convert first character of string literal type to uppercase
 */
type Capitalize<S extends string> = intrinsic;

export type StudentSexType = 'male' | 'female'

const studentSex: Capitalize<StudentSexType> = 'Male'
```

##### 17. Uncapitalize（首字母小写）
```ts
/**
 * Convert first character of string literal type to lowercase
 */
type Uncapitalize<S extends string> = intrinsic;

export type StudentSexType = 'MALE' | 'FEMALE'

const studentSex: Uncapitalize<StudentSexType> = 'mALE'
```

----

[你不知道的 TypeScript 高级类型](https://juejin.cn/post/6865860467307315207)  
[TypeScript内置类型一览](https://juejin.cn/post/7040300769072906277)   
