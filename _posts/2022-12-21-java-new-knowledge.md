---
title: Java新特性
key: 2022-12-21
tags: java
---

### 1. Stream
##### 1. 什么是 Stream
Stream（流）是一个来自数据源的元素队列并支持聚合操作
- 元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
- 数据源 流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。
- 聚合操作 类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。

##### 2. 生成流
- stream() − 为集合创建串行流。
- parallelStream() − 为集合创建并行流。

##### 3. 常用方法
```java
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);

// forEach
numbers.stream().forEach(System.out::println);

// map
List<Integer> squaresList = numbers.stream().map( i -> i*i).collect(Collectors.toList());

// filter
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
// 获取空字符串的数量
long count = strings.stream().filter(string -> string.isEmpty()).count()

// distinct 去重
List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
```

### 2. Lambda 表达式
是一个匿名函数，是对匿名函数的简写形式，我们可以把 Lambda表达式理解为是一段可以传递的代码（将代码像数据一样进行传递），可以写出更简洁、更灵活的代码

##### 1. 语法
`(parameters) -> expression`

##### 2. 实例
```java
public class Java8Tester {
   public static void main(String args[]){
      Java8Tester tester = new Java8Tester();
        
      // 类型声明
      MathOperation addition = (int a, int b) -> a + b;
        
      // 不用类型声明
      MathOperation subtraction = (a, b) -> a - b;
        
      // 大括号中的返回语句
      MathOperation multiplication = (int a, int b) -> { return a * b; };
        
      // 没有大括号及返回语句
      MathOperation division = (int a, int b) -> a / b;
        
      System.out.println("10 + 5 = " + tester.operate(10, 5, addition));
      System.out.println("10 - 5 = " + tester.operate(10, 5, subtraction));
      System.out.println("10 x 5 = " + tester.operate(10, 5, multiplication));
      System.out.println("10 / 5 = " + tester.operate(10, 5, division));
        
      // 不用括号
      GreetingService greetService1 = message ->
      System.out.println("Hello " + message);
        
      // 用括号
      GreetingService greetService2 = (message) ->
      System.out.println("Hello " + message);
        
      greetService1.sayMessage("Runoob");
      greetService2.sayMessage("Google");
   }
    
   interface MathOperation {
      int operation(int a, int b);
   }
    
   interface GreetingService {
      void sayMessage(String message);
   }
    
   private int operate(int a, int b, MathOperation mathOperation){
      return mathOperation.operation(a, b);
   }
}


```

### 3. 双冒号（::）
双冒号（::）运算符在Java 8中被用作方法引用（method reference)
- 静态方法引用（static method）语法：classname::methodname 例如：Person::getAge
- 对象的实例方法引用语法：instancename::methodname 例如：System.out::println
- 对象的超类方法引用语法： super::methodname
- 类构造器引用语法： classname::new 例如：ArrayList::new
- 数组构造器引用语法： typename[]::new 例如： String[]:new

```java
public class Colon{
    @Test
    public void test(){
        List<String> list = Arrays.asList("a","b","c");
        //静态方法引用  ClassName::methodName
        list.forEach(Colon::print);
        //上一行等价于
        //list.forEach((x)->Colon.print(x));
    }
    //静态方法
    public static void print(String s){
        System.out.println(s);
    }
}

public class Colon{
    @Test
    public void test(){
        String[] arr = { "Barbara", "James", "Mary", "John",
                "Patricia", "Robert", "Michael", "Linda" };
        //引用String类型的任意对象的compareToIgnoreCase方法实现忽略大小写排序
        Arrays.sort(arr, String::compareToIgnoreCase);
        //上一行等价于
        //Arrays.sort(arr, (a,b)->a.compareToIgnoreCase(b));
        //输出
        for(String s:arr){
            System.out.println(s);
        }
    }
}
```
### 4. Optional
Optional 类是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。   
Optional 是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。   
Optional 类的引入很好的解决空指针异常。

```java
public class Java8Tester {
   public static void main(String args[]){
   
      Java8Tester java8Tester = new Java8Tester();
      Integer value1 = null;
      Integer value2 = new Integer(10);
        
      // Optional.ofNullable - 允许传递为 null 参数
      Optional<Integer> a = Optional.ofNullable(value1);
        
      // Optional.of - 如果传递的参数是 null，抛出异常 NullPointerException
      Optional<Integer> b = Optional.of(value2);
      System.out.println(java8Tester.sum(a,b));
   }
    
   public Integer sum(Optional<Integer> a, Optional<Integer> b){
    
      // Optional.isPresent - 判断值是否存在
        
      System.out.println("第一个参数值存在: " + a.isPresent());
      System.out.println("第二个参数值存在: " + b.isPresent());
        
      // Optional.orElse - 如果值存在，返回它，否则返回默认值
      Integer value1 = a.orElse(new Integer(0));
        
      //Optional.get - 获取值，值需要存在
      Integer value2 = b.get();
      return value1 + value2;
   }
}
```

### 5. java10 var类型
`var 变量名 = 初始值；`   
- var不是关键字，它相当于是一种动态类型；
- var动态类型是编译器根据变量所赋的值来推断类型；
- var 没有改变Java的本质，var只是一种简便的写法，

##### 1. 注意事项
- var只能在方法内定义变量，不允许定义类的成员变量。
- var 定义变量必须赋初始值，以后不能在赋初始值。
- var每次只能定义一个变量，不能复合声明变量。

----

[Java 8 Stream](https://www.runoob.com/java/java8-streams.html)  
[深入理解Java双冒号(::)运算符的使用](https://blog.csdn.net/zhoufanyang_china/article/details/87798829)  
[Java Lambda 表达式](https://www.runoob.com/java/java8-lambda-expressions.html)  
[Java 8 Optional 类](https://www.runoob.com/java/java8-optional-class.html)  
