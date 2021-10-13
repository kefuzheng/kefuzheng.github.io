---
title: Go基础知识
key: 2021-10-11
tags: Go
---

### 1. Go常用命令
- go version
- go get [url] 动态获取远程代码包到GOPATH路径
- go build 编译源码文件、代码包、依赖包
- go run 编译并运行go源码文件

### 2. 基础结构
```go
// 程序所属包，必须在开头
package main
// 导入依赖包
import "fmt"
// 常量定义
const NAME string = "Kefu"
// 全局变量的声明与赋值
var a string = "test"
// 一般类型声明
type testInt int
// 结构的声明
type Learn struct {
}
// 声明接口
type Ilearn interface {
}
// 函数定义
func learn() {
}
// main()函数
func main() {
  fmt.Println("Hello Go")
}
```
##### 1. package
- 要生成go语言可执行程序，必须要有main的package包，且必须在该包下有mian()函数
- 同一路径下只能存在一个package

##### 2. import
- 如果一个mian导入其他包，包将被顺序导入
- 如果导入的包中依赖其它包（包B），会首先导入B包，然后初始化B包中常量和变量，最后如果B包中有init，会自动执行init()
- 所有包导入完成后才会对main中常量和变量进行初始化，然后执行main中的init函数（如果存在），最后执行main函数
- 如果一个包被导入多次，则该包只会被导入一次
- `import test "fmt"` 别名
- `import . "fmt"` 调用该包中函数时可以省略前缀包（不推荐这样使用，容易产生冲突）
- `import _ "test"` 导入该包，但不导入整个包，而是执行该包中的init函数（常量和变量会在init前执行，因此也初始化了它们），因此无法通过包名来调用包中的其它函数

### 3. 数据类型
数据类型的出现是为了把数据分成所需内存大小不同的数据，编程的时候需要用大数据的时候才需要申请大内存，就可以充分利用内存   
![数据类型](/assets/images/data_type.PNG)   
![数字类型](/assets/images/number_type.PNG)   
![浮点类型](/assets/images/float_type.PNG)   
- 类型零值（默认值）：值类型的默认值为0，布尔类型默认值为false，string默认值为空字符串
- 类型设置别名：`type test int32`

![数据存储大小](/assets/images/data_storage.PNG)   


----


