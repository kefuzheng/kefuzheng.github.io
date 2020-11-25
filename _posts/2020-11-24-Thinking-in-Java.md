---
title: Thinking in Java
key: 2020-11-24
tags: Java
---

### 1. 一切都是对象
##### 1. 用引用操纵对象
尽管一切都看作对象，但操纵的标识符实际上是对象的一个“引用”（reference）。可以想象成遥控器（引用）来操纵电视机（对象）。   
如果想操纵一个词或句子。则可以创建一个String引用：   
`String s;`   
但**这里所创建的只是引用，并不是对象**。
##### 2. 必须有你创建所有对象
一旦创建了一个引用，就希望它能与一个新的对象相关联。通常用new操作符来实现这一目的。new关键字的意思是“给我一个新对象”，所以前面的例子可以写成：   
`String s = new String("asdf);`   
##### 3. 存储到什么地方
1. 寄存器：这是最快的存储区，因为它位于不同于其他存储区的地方--处理器内部。但是寄存器的数量极其有限，所以寄存器根据需求进行分配。你不能直接控制，也不能在程序中感觉到寄存器存在的任何迹象（C和C++允许您想编译器建议寄存器的分配方式）
2. 堆栈：位于通用RAM（随机访问存储器）中，但通过**堆栈指针**可以从处理器那里获得直接支持。堆栈指针若向下移动，则分配新的内存；若向上移动，则释放那些内存。这是一种快速有效的分配存储方法，仅次于寄存器。创建程序时，Java系统必须知道存储在堆栈内所有项的确切生命周期，以便上下移动堆栈指针。这一约束限制了程序的灵活性，所以虽然**某些Java数据存储于堆栈中**--特别时对象引用，但是Java对象并不存储于其中。
3. 堆：一种通过的内存池（也位于RAM区），用于**存放所有的Java对象**。堆不同于堆栈的好处是：编译器不需要知道存储的数据在堆里存活多长时间。因此，在堆里分配存储有很大的灵活性。当需要一个对象时，只需要new写一行简单的代码，当执行这行代码时，会自动在堆里进行存储分配。当然，为这种灵活性必须付出相应的代价：用堆进行存储分配和清理可能比用堆栈进行存储分配需要更多的时间。
4. 常量存储：常量值通常直接存放在程序代码内部，这样做是安全的，因为它们永远不会被改变。有时，在嵌入式系统中，常量本身会和其他部分隔离开，所以在这种情况下，可以选择将其存放在ROM（只读存储器）中。
5. 非RAM存储：如果数据完全存活于程序之外，那么它可以不受程序的任何控制，在程序没有运行时也可以存在。其中两个基本的例子是**流对象和持久化对象**。在流对象中，对象转化成字节流，通常被发送给另一台机器。在“持久化对象”中，对象被存放于磁盘上，因此，即使程序终止，他们仍可以保持自己的状态。这种存储方式的技巧在于：把对象转化成可以存放在其他媒介上的事物，在需要时，可恢复成常规的、基于RAM的对象

##### 4. 基本类型
对于基本类型Java不用new来创建变量，而是创建一个并非是引用的“自动”变量，这个变量直接存储“值”，并置于堆栈中，因此更加高效。   
Java要确定每种基本类型所占存储空间的大小。**它们的大小并不像其他大多数语言那样随机器硬件架构的变化而变化**。这种所占存储空间大小的不变性是Java程序比用其他大多数语言编写的程序更具可移植性的原因之一。  
|基本类型|大小|最小值|最大值|包装器类型|
|----|----|----|----|----|----|
|boolean|-|-|-|Boolean|
|char|16-bit|Unicode 0|Unicode 2^16-1|Character|
|byte|8 bits|-128|+127|Byte|
|short|16 bits|-2^15|+2^15-1|Short|
|int|32 bits|-2^31|+2^31-1|Integer|
|long|64 bits|-2^63|+2^63-1|Long|
|float|32 bits|IEEE754|IEEE754|Float|
|double|64 bits|IEEE754|IEEE754|Double|

boolean类型所占存储空间的大小没有明确指定，仅定义为能够取字面值true或false
##### 5. 数组
当创建一个数组对象时，实际上就是对创建了一个引用数组，并且每个引用都会自动被初始化一个特定值，该值拥有自己的关键字null。一旦Java看到null，就知道这个引用还没有指向某个对象。在使用任何引用前，必须为其指定一个对象；如果试图使用一个还是null的引用，在运行时将会报错。
##### 6. 基本成员默认值
若类的某个成员是基本数据类型，即使没有进行初始化，Java也会确保它获得一个默认值（boolean:false, char:'\uoooo'(null), byte:(byte)0, short:(short)0, int:0, long:oL, float:0.0f, double:0.0d）  
然而上述**确保初始化的方法并不适用于“局部”变量**（即并非某个类的字段）。因此，如果在某个方法定义中有   
`int x;`  
那么变量x得到的可能是任意值，而不会被自动初始化为零。
##### 7. 参数列表
在方法的参数列表中必须指定每个所传递对象的类型及名字。对于非基本类型，像Java中任何传递对象的场合一样，这里传递的实际上也是引用。
### 2. 操作符
##### 1. 赋值=
赋值操作符的意思是“取右边的值，把它复制给左边”。
- 基本类型：基本类型存储了实际的数值，而并非指向一个对象的引用，所以在位其赋值的时候，是直接将一个地方的内容复制到了另一个地方。例如，对基本数据类型使用a=b，那么b的内容就复制给a，若接着又修改了a，而b根本不会受这种修改的影响。
- 对象赋值：对一个对象进行操作时，我们真正操作的是对对象的引用。所以倘若“将一个对象赋值给另一个对象”，实际是将“引用”从一个地方复制到另一个地方。这意味着假如对对象使用c=d，那么c和d都指向原本只有d指向的哪个对象。

##### 2. 算术操作符
加号（+），减号（-），乘号（*），除号（/）以及取模（%取余数）  
自动递增和递减：  
- 前缀式：对于前缀递增和前缀递减（++a或--a），会先执行运算，再生成值
- 后缀式：对于后缀递增和后缀递减（a++或a--），会先生成值，再执行运算

```java
int i = 1;
print("i: " + i);       // i :1
print("++i: " + ++i);   // ++i: 2
print("i++: " + i++);   // i++: 2
print("i: " + i);       // i: 3
print("--i: " + --i);   // --i: 2
print("i--: " + i--);   // i--: 2
print("i: " + i);       // i: 1
```
##### 3. 按位操作符
按位操作符是用来操作整数基本数据类型中的单个比特，即二进制位。
1. 与运算（&）：两位同时为“1”，结果才为“1”，否则为0。例如：3&5  即 0000 0011 & 0000 0101 = 0000 0001   因此，3&5的值得1。
2. 或运算（`|`）：参加运算的两个对象只要有一个为1，其值为1。例如:`3|5　即 0000 0011 | 0000 0101 = 0000 0111`,因此，`3|5`的值得7。
3. 异或运算（^）：参加运算的两个对象，如果两个相应位为“异”（值不同），则该位结果为1，否则为0。例如：9^5可写成算式如下： 00001001^00000101=00001100 (十进制为12)可见9^5=12  
4. 按位非（~）：取反操作符
   
##### 4. 字符串
String对象是不可变的，String类中每一个看起来会修改String值的方法，实际上都是创建了一个全新的String对象。
##### 5. 数组
无论使用哪种类型的数组，数组标识符其实只是一个引用，指向再堆中创建的一个真实对象，这个（数组）对象用以保存指向其他对象的引用。可以作为数组初始化语法的一部分隐式地创建此对象，或者用new表达式显示地创建。“[]”语法是访问数组对象唯一的方式。   
对象数组保存的是引用，基本类型数组直接保存基本类型的值。
### 3. 访问权限控制
权限控制的大小情况是这样的：public > protected > default(包访问权限) > private   
访问权限|本类|本包的类|子类|非子类的外包类  
public|是|是|是|是  
protected|是|是|是|否  
default|是|是|否|否  
private|是|否|否|否  

### 4. 泛型
##### 1. 泛型的定义以及存在意义
泛型，即“参数化类型”。就是将类型由原来的具体的类型参数化，类似于方法中的变量参数，此时类型也定义成参数形式（可以称之为类型形参），然后在使用/调用时传入具体的类型（类型实参）。   
一些常用的泛型类型变量：   
- E：元素（Element），多用于java集合框架
- K：关键字（Key）
- N：数字（Number）
- T：类型（Type）
- V：值（Value）

泛型适用于多种数据类型执行相同的代码（代码复用）；泛型中的类型在使用时指定，不需要强制类型转换（类型安全，编译器会检查类型）
##### 2. 泛型类的使用
```java
public class GenericClass<T> {
    private T data;

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public static void main(String[] args) {
        GenericClass<String> genericClass=new GenericClass<String>();
        genericClass.setData("Generic Class");
        System.out.println(genericClass.getData());
    }
}
```
##### 3. 泛型接口的使用
```java
// 泛型接口
public interface GenericIntercace<T> {
     T getData();
}

// 实现泛型接口一
public class ImplGenericInterface1<T> implements GenericIntercace<T> {
    private T data;

    private void setData(T data) {
        this.data = data;
    }

    @Override
    public T getData() {
        return data;
    }

    public static void main(String[] args) {
        ImplGenericInterface1<String> implGenericInterface1 = new ImplGenericInterface1<String>();
        implGenericInterface1.setData("Generic Interface1");
        System.out.println(implGenericInterface1.getData());
    }
}

// 实现泛型接口二
public class ImplGenericInterface2 implements GenericIntercace<String> {
    @Override
    public String getData() {
        return "Generic Interface2";
    }

    public static void main(String[] args) {
        ImplGenericInterface2 implGenericInterface2 = new ImplGenericInterface2();
        System.out.println(implGenericInterface2.getData());
    }
}
```
##### 4. 泛型方法的使用
```java
public class GenericMethod1 {
    private static int add(int a, int b) {
        System.out.println(a + "+" + b + "=" + (a + b));
        return a + b;
    }

    // 有返回值
    private static <T> T genericAdd(T a, T b) {
        System.out.println(a + "+" + b + "="+a+b);
        return a;
    }

    // 没返回值
    public void show01(T t) {
        System.out.println(t.toString());
    }

    public <T> void show02(T t) {
        System.out.println(t.toString());
    }

    public static void main(String[] args) {
        GenericMethod1.add(1, 2);
        GenericMethod1.<String>genericAdd("a", "b");
    }
}
```
##### 5. 限定泛型类型变量
```java
// 类型变量的限定-类  限定类型使用extends关键字指定,可以使类，接口，类放在前面接口放在后面用&符号分割
public class TypeLimitForClass<T extends List & Serializable> {
    private T data;

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public static void main(String[] args) {
        ArrayList<String> stringArrayList = new ArrayList<>();
        stringArrayList.add("A");
        stringArrayList.add("B");
        ArrayList<Integer> integerArrayList = new ArrayList<>();
        integerArrayList.add(1);
        integerArrayList.add(2);
        integerArrayList.add(3);
        TypeLimitForClass<ArrayList> typeLimitForClass01 = new TypeLimitForClass<>();
        typeLimitForClass01.setData(stringArrayList);
        TypeLimitForClass<ArrayList> typeLimitForClass02 = new TypeLimitForClass<>();
        typeLimitForClass02.setData(integerArrayList);

        System.out.println(getMinListSize(typeLimitForClass01.getData().size(), typeLimitForClass02.getData().size()));
}

    // 类型变量的限定-方法
    public static <T extends Comparable<T>> T getMinListSize(T a, T b) {
        return (a.compareTo(b) < 0) ? a : b;
    }
}
```
##### 6. 泛型中的约束和局限性
1. 不能实例化泛型类
2. 静态变量或方法不能引用泛型类型变量，但是静态泛型方法是可以的
3. 基本类型无法作为泛型类型
4. 无法使用instanceof关键字或==判断泛型类的类型
5. 泛型类的原生类型与所传递的泛型无关，无论传递什么类型，原生类是一样的
6. 泛型数组可以声明但无法实例化
7. 泛型类不能继承Exception或者Throwable
8. 不能捕获泛型类型限定的异常但可以将泛型限定的异常抛出

##### 7. 通配符类型
1. <? extends Parent> 指定了泛型类型的上届:`public static void getUperNumber(List<? extends Number> data)`
2. <? super Child> 指定了泛型类型的下届, 如List<? super Number>来定义，表示类型只能接受Number及其三层父类类型，如 Object 类型的实例。
3. <?> 指定了没有限制的泛型类型

### 5. Deque
Deque是double ended queue的简称，习惯上称之为双端队列。大多数Deque 实现对它们可能包含的元素的数量没有固定的限制，但是该接口支持容量限制的deques以及没有固定大小限制的deque。  
与List接口不同，此接口不支持索引访问元素。   
- LinkedList 大小可变的链表双端队列，允许元素为插入null。
- ArrayDeque 大下可变的数组双端队列，不允许插入null。
- ConcurrentLinkedDeque 大小可变且线程安全的链表双端队列，非阻塞，不允许插入null。
- LinkedBlockingDeque 为线程安全的双端队列，在队列为空的情况下，获取操作将会阻塞，直到有元素添加。


----

[与、或、异或运算](https://www.cnblogs.com/wisdom-jie/p/7732940.html)   
[Java泛型详解](https://www.jianshu.com/p/986f732ed2f1)   
[深入理解Java集合之---Deque](https://www.jianshu.com/p/d78a7c982edb)    