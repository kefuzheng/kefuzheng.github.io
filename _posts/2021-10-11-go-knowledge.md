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
- go list -m all 列出依赖

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

##### 1. 变量
- 变量的声明格式：`var <变量名称> [变量类型]`
- 声明和赋值同时进行：`var <变量名称> [变量类型] = <值>` 或 `<变量名称> := <值>`（只能用在函数体内）
- 全局变量的声明必须使用var关键字，局部变量则可以省略
- 特殊变量下划线`_`，把值赋值给下划线，就不再需要整个值，进行回收
- Go中不存在隐式转换，类型转换必须是显示的，且只能发生在两种兼容类型之间：`<变量名称> [:]= <目标类型>(<需要转换的变量>)`
- 变量的可见性规则：大写字母开头的变量是可导出的（公用变量），小写字母开头的就是不可导出的（私有变量）

##### 2. 常量
定义：   
显示：`const name [type] = value`   
隐式（无类型变量）：`const name = value`   
常量可以使用内置表达式定义，例如：len(), unsafe.SizeOf()等   
常量只支持 布尔类型、数字类型和字符串类型

##### 3. 特殊常量iota
- iota只能在常量中使用
- iota在const关键字出现时将被重置为0；
- const体中每新增一行常量声明将使iota计数一次；
- iota常见使用方法：跳值使用法，插队使用法，表达式隐式使用法，单行使用法

```go
//跳值使用法
const (
  a = iota
  b = iota
  -
  c = iota
)
** a,b,c = 0,1,3

//插队使用法
const (
  a = iota
  b = 3.14
  c = iota
)
** a,b,c = 0,3.14,2

//表达式隐式使用法， 当没有使用表达式的时候，会自动向上使用第一个非空的表达式
const (
  a = iota * 2
  b
  c
)
** a,b,c = 0,2,4

//单行使用法
const (
  a,b = iota+1, iota+3
  c,d
  f = iota
)
** a,b,c,d,f = 1,3,2,4,2
```

##### 5. 数组
数组的长度是不可变的
```go
var variable_name [SIZE] variable_type
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
// 数组长度不确定，可以使用 ... 代替数组的长度，编译器会根据元素个数自行推断数组的长度
balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
// 将索引为 1 和 3 的元素初始化
balance := [5]float32{1:2.0,3:7.0}
```

##### 6. 切片（Slice）
切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大，切片可看作动态数组   
```go
// 定义
// 声明一个未指定大小的数组来定义切片，切片不需要说明长度
var slice0 []type
s0 :=[] int {1,2,3 } 

// len 是数组的长度并且也是切片的初始长度。
var slice1 []type = make([]type, len)
slice1 := make([]type, len)

// 指定容量，其中capacity是可选参数
make([]T, length, capacity)
```
- len() 方法获取长度`len(slice)`
- cap() 可以测量切片最长可以达到多少`cap(slilce)`
- 空(nil)切片: 一个切片在未初始化之前默认为 nil，长度为 0

```go
/* 创建切片 */
numbers := []int{0,1,2,3,4,5,6,7,8}

/* 打印原始切片 */
fmt.Println("numbers ==", numbers)

/* 打印子切片从索引1(包含) 到索引4(不包含)*/
fmt.Println("numbers[1:4] ==", numbers[1:4])

/* 默认下限为 0*/
fmt.Println("numbers[:3] ==", numbers[:3])

/* 默认上限为 len(s)*/
fmt.Println("numbers[4:] ==", numbers[4:])


var numbers []int

/* 允许追加空切片 */
numbers = append(numbers, 0)

/* 向切片添加一个元素 */
numbers = append(numbers, 1)

/* 同时添加多个元素 */
numbers = append(numbers, 2,3,4)

/* 创建切片 numbers1 是之前切片的两倍容量*/
numbers1 := make([]int, len(numbers), (cap(numbers))*2)

/* 拷贝 numbers 的内容到 numbers1 */
copy(numbers1,numbers)
```
##### 7. Map
```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)

var countryCapitalMap map[string]string /*创建集合 */
countryCapitalMap = make(map[string]string)

/* map插入key - value对,各个国家对应的首都 */
countryCapitalMap [ "France" ] = "巴黎"
countryCapitalMap [ "Italy" ] = "罗马"

delete(countryCapitalMap, "France")
```

### 4. 运算与控制
##### 1. 按位运算符
![按位运算](/assets/images/bit_cal.PNG)   
##### 2. for
```go
a := []string{"apple","banana","peach"}
for index, value := range a {
  fmt.Print(index)
  fmt.Print(value)
}
// 若不需要index，将index改为_
```
##### 3. goto
跳到指定代码块
```go
goto One
fmt.Print("block 0")
One:
  fmt.Print("block 1")
```

### 5. 内建方法
##### 1. make
- 创建slice、map、chan
- 返回引用类型

##### 2. new
- 内存置零
- 返回传入类型的指针地址

##### 3. copy
不会扩容，不会改变目标切片的长度

##### 4. panic & recover
- panic 抛出异常
- recover 捕获异常

```go
func receivePanic(){
  defer coverPanic()
  panic(errors.New("I am error"))
}

func coverPanic(){
  message := recover()
  switch message.(type) {
  case string:
    fmt.Println("string message: ", message)
  case error:
    fmt.Println("error message: ", message)
  default:
    fmt.Println("unknow message: ", message)
  }
}
```

##### 5. close
关闭chan
```go
mChan := make(chan int, 1)
// 当写完数据后，关闭chan
defer close(mChan)
mChan <- 1
```

### 6. 函数
##### 1. 定义
```go
func max(num1, num2 int) int {
   /* 声明局部变量 */
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result
}

// 函数返回多个值
func swap(x, y string) (string, string) {
   return y, x
}

func main() {
   a, b := swap("Google", "Runoob")
   fmt.Println(a, b)
}
```

##### 2. 函数参数
- 值传递:值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
- 引用传递:引用传递是指在调用函数时将实际参数的地址传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。

```go
import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int= 200

   fmt.Printf("交换前，a 的值 : %d\n", a )
   fmt.Printf("交换前，b 的值 : %d\n", b )

   /* 调用 swap() 函数
   * &a 指向 a 指针，a 变量的地址
   * &b 指向 b 指针，b 变量的地址
   */
   swap(&a, &b)

   fmt.Printf("交换后，a 的值 : %d\n", a )
   fmt.Printf("交换后，b 的值 : %d\n", b )
   // 交换前，a 的值 : 100
   // 交换前，b 的值 : 200
   // 交换后，a 的值 : 200
   // 交换后，b 的值 : 100
}

func swap(x *int, y *int) {
   var temp int
   temp = *x    /* 保存 x 地址上的值 */
   *x = *y      /* 将 y 值赋给 x */
   *y = temp    /* 将 temp 值赋给 y */
}
```

##### 3. 函数作为实参
```go
func main(){
   /* 声明函数变量 */
   getSquareRoot := func(x float64) float64 {
      return math.Sqrt(x)
   }

   /* 使用函数 */
   fmt.Println(getSquareRoot(9))
}
```

##### 4. 函数闭包
```go
func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
   // 1
   // 2
   // 3
   // 1
   // 2
}
```

##### 5. 可变参数
```go
func test(values...string) {
  for _,v := range values {
    fmt.Println("---> v:", v)
  }
}
```

### 7. 结构体struct和接口interface
```go
// 定义结构体
type Dog struct {
  id int  //属性
  name string
  age int
}

// 方法, 通过方法首字母的大小写，来控制访问权限，大写对包外可见，小写不可见
func (d *Dog) Run() {
  fmt.Println(d)
}

func testForStruct() {
  // 方式一
  var dog Dog
  dog.id = 0
  dog.name = "KiKi"
  dog.age = 3
  fmt.Println("dog: ",dog)

  // 方式二
  dog1 := Dog{id:1, name:"Yaya", age:2}
  fmt.Println("dog1: ",dog1)
  
  // 方式三 定义结构体的指针类
  dog3 := new(Dog) // var dog3 *Dog
  dog3.id = 3
  dog3.name = "Nana"
  dog3.age = 4
  fmt.Println("dog3: ",dog3) 

  // 查看结构体地址，将&符号放置于结构体变量前
  &dog3
}
```

##### 1. 继承
```go
type Animal struct {
  colour string
}

// 定义结构体
type Dog struct {
  Animal
  id int  //属性
  name string
  age int
}

func (a *Animal) Run() {
  fmt.Println(a)
}

func (d *Dog) Run() {
  fmt.Println(d)
}
```

##### 2. 接口interface
把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口
```go
type Phone interface {
    call()
}

type NokiaPhone struct {
}

func (nokiaPhone NokiaPhone) call() {
    fmt.Println("I am Nokia, I can call you!")
}

type IPhone struct {
}

func (iPhone IPhone) call() {
    fmt.Println("I am iPhone, I can call you!")
}

func main() {
    var phone Phone

    phone = new(NokiaPhone)
    phone.call()

    phone = new(IPhone)
    phone.call()

}

// I am Nokia, I can call you!
// I am iPhone, I can call you!
```

### 8. 并发
##### 1. goroutine 协程
goroutine 是轻量级线程，goroutine 的调度是由 Golang 运行时进行管理的。
`go 函数名( 参数列表 )` //开启协程   
##### 2. 限制所使用的CPU核心数
`runtime.GOMAXPROCS(runtime.NumCPU() - 1)` //最大核心数减一    
##### 3. 协程间通信channel
通道（channel）是用来传递数据的一个数据结构。   
通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 <- 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。
```go
ch := make(chan int) // 声明通道
ch <- v    // 把 v 发送到通道 ch
v := <-ch  // 从 ch 接收数据
           // 并把值赋给 v
```
带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，就是说发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据。   
不过由于缓冲区的大小是有限的，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。   
如果通道不带缓冲，发送方会阻塞直到接收方从通道中接收了值。如果通道带缓冲，发送方则会阻塞直到发送的值被拷贝到缓冲区内；如果缓冲区已满，则意味着需要等待直到某个接收方获取到一个值。接收方在有值可以接收之前会一直阻塞。   
```go
var chanInt chan int = make(chan int, 10)
var timeout chan bool = make(chan bool)

func Send() {
  time.Sleep(time.Second * 1)
  chanInt <- 1
  time.Sleep(time.Second * 1)
  chanInt <- 2
  time.Sleep(time.Second * 1)
  chanInt <- 3
  time.Sleep(time.Second * 1)
  timeout <- true
}

func Receive() {
  for{
    select {
    case num := <-chanInt:
      fmt.Println("num:", num)
    case <-timeout:
      fmt.Println("tiemout")
    }
  }
}

go Send()
go Receive()
time.Sleep(time.Second * 60)
```
##### 4. 协程同步
系统工具sync.WaitGroup   
- Add(delta int) 添加协程记录
- Done() 移除协程记录
- Wait() 同步等待所有记录的协程全部结束

```go
var WG sync.WaitGroup

func Read() {
  for i:=0;i<3;i++ {
    WG.Add(delta:1)
  }
}

func Write() {
  for i:=0;i<3;i++ {
    time.Sleep(time.Second * 2)
    fmt.Println("Done->", i)
    WG.Done()
  }
}

Read()
go Write()
WG.Wait()
fmt.Println("All Done")
time.Sleep(time.Second * 60)
```

### 9. 指针
- 取地址符是 &，放到一个变量前使用就会返回相应变量的内存地址
- `*`号用于指定变量是作为一个指针
- 指针类型前面加上 * 号（前缀）来获取指针所指向的内容
- nil 指针也称为空指针
- go语言不支持指针运算

```go
var a int= 20   /* 声明实际变量 */
var ip *int     /* 声明指针变量 */

ip = &a  /* 指针变量的存储地址 */

fmt.Printf("a 变量的地址是: %x\n", &a  )

if ip != nil {
  /* 指针变量的存储地址 */
  fmt.Printf("ip 变量储存的指针地址: %x\n", ip )
  
  /* 使用指针访问值 */
  fmt.Printf("*ip 变量的值: %d\n", *ip )
}

// a 变量的地址是: 20818a220
// ip 变量储存的指针地址: 20818a220
// *ip 变量的值: 20
```

##### 1. 数组和指针
```go
// 指针数组
a,b := 1,2
pointArr := [...]*int{&a, &b}
fmt.Println("指针数组 pointArr:", pointArr)

// 数组指针
arr := [...]int{3,4,5}
arrPoint := &arr
fmt.Println("数组指针 arrPoint:", arrPoint)
```

##### 2. 指向指针的指针
如果一个指针变量存放的又是另一个指针变量的地址，则称这个指针变量为指向指针的指针变量。   
```go
var a int
var ptr *int
var pptr **int

a = 3000

/* 指针 ptr 地址 */
ptr = &a

/* 指向指针 ptr 地址 */
pptr = &ptr

/* 获取 pptr 的值 */
fmt.Printf("变量 a = %d\n", a )
fmt.Printf("指针变量 *ptr = %d\n", *ptr )
fmt.Printf("指向指针的指针变量 **pptr = %d\n", **pptr)

// 变量 a = 3000
// 指针变量 *ptr = 3000
// 指向指针的指针变量 **pptr = 3000
```

### 10. JSON
##### 1. 序列化
```go
type Server struct {
  ServerName string `json:"name"` // Tag类似于注解
  ServerIp string `json:ip`
  ServerPort int `json:"port"`
}

func SerializeStruct() {
  server := new(Server)
  server.ServerName = "Demo-for-json-struct"
  server.ServerIp = "127.0.0.1"
  server.ServerPort = 8080
  serialize(server)
  // {"name":"Demo-for-json-struct", "ip":"127.0.0.1", "port":8080}
}

func SerializeMap() {
  server := make(map[string]interface{})
  server["ServerName"] = "Demo-for-json-map"
  server["ServerIp"] = "128.0.0.1"
  server["ServerPort"] = 9090
  serialize(server)
  // {"ServerName":"Demo-for-json-map", "ServerIp":"128.0.0.1", "ServerPort":9090}
}

func serialize(server interface{}) {
  b, err := json.Marshal(server)  //序列化成json字节数组
  if err != nil {
    fmt.Println("Marshal err:", err.Error())
    return
  }
  fmt.Println("Marshal json:", string(b))  //将json字节数组转换为string
}
```

##### 2. 反序列化
```go
func DeSerializeStruct() {
  jsonString := "{"name":"Demo-for-json-struct", "ip":"127.0.0.1", "port":8080}"
  server := new(Server)
  err := json.Unmarshall([]byte(jsonString), &server)
  if err != nil {
    fmt.Println("unmarshal err:", err.Error())
    return
  }
  fmt.Println("unmarshal struct:", server)
}

func DeSerializeMap() {
  jsonString := "{"name":"Demo-for-json-struct", "ip":"127.0.0.1", "port":8080}"
  server := make(map[string]interface{})
  err := json.Unmarshall([]byte(jsonString), &server)
  if err != nil {
    fmt.Println("unmarshal err:", err.Error())
    return
  }
  fmt.Println("unmarshal map:", server)
}
```

### 11. Module
Go module是版本管理的依赖项的包管理工具   
- go mod init module_name // 初始化
- go mod graph // 输出当前module所有的依赖
- go mod download // 下载所依赖的包
- go mod tidy  // 整理依赖，把需要的依赖添加进来，不需要的删除
- go mod verify // 验证依赖：版本，源代码等
- go mod why // 展示依赖关系
- go mod edit //修改god.mod的相关内容，例如-require=path@version 获取依赖
- go mod vendor // 将所有的依赖，放到当前工程的vendor目录下


----

[Go 语言教程](https://www.runoob.com/go/go-tutorial.html)   

