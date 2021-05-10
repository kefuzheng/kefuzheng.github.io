---
title: java知识
key: 2019-05-22
tags: Java
---

### 1. java 中 byte[]、File、InputStream 互相转换
##### 1. 将File、FileInputStream 转换为byte数组：
```java
File file = new File("test.txt");
InputStream input = new FileInputStream(file);
byte[] byt = new byte[input.available()];
input.read(byt);
```
##### 2. 将byte数组转换为InputStream：
```java
byte[] byt = new byte[1024];
InputStream input = new ByteArrayInputStream(byt);
```
##### 3. 将byte数组转换为File：
```java
File file = new File('');
OutputStream output = new FileOutputStream(file);
BufferedOutputStream bufferedOutput = new BufferedOutputStream(output);
bufferedOutput.write(byt);
```
### 2. 文件最后修改时间
`Long a=file.lastModified();`
### 3. 查看class编译的版本
`javap -v Test.class | grep major`   
J2SE 8 = 52,   
J2SE 7 = 51,   
J2SE 6.0 = 50,   
J2SE 5.0 = 49,   
JDK 1.4 = 48,   
JDK 1.3 = 47,   
JDK 1.2 = 46,  
JDK 1.1 = 45  
### 4. Java创建文件并写入内容
```java
public void createFile() throws IOException {
    String filePath = "D:/a/b";
    File dir = new File(filePath);
    // 一、检查放置文件的文件夹路径是否存在，不存在则创建
    if (!dir.exists()) {
        dir.mkdirs();// mkdirs创建多级目录
    }
    File checkFile = new File(filePath + "/filename.txt");
    FileWriter writer = null;
    try {
        // 二、检查目标文件是否存在，不存在则创建
        if (!checkFile.exists()) {
            checkFile.createNewFile();// 创建目标文件
        }
        // 三、向目标文件中写入内容
        // FileWriter(File file, boolean append)，append为true时为追加模式，false或缺省则为覆盖模式
        writer = new FileWriter(checkFile, true);
        writer.append("your content");
        writer.flush();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (null != writer)
            writer.close();
    }
}
```
### 5. double型转化成int型
一个变量 double x；  
不进行四舍五入操作：  
(int)x   
进行四舍五入操作：  
Integer.parseInt(new java.text.DecimalFormat("0.00").format(x))  
### 6. FileWriter(Java)以UTF-8格式编写文件
`FileWriter fw = new FileWriter("filename.txt", Charset.forName("utf-8"));`  java11   
`Writer out = new BufferedWriter( new OutputStreamWriter(new FileOutputStream(this.outputFilename),”UTF-8″));`   old java   
### 7. 奇偶数的判断
(1) 通过a%2！=0来判断。  
(2) 通过(a&1)==1来判断。（推荐使用，位操作，性能更优）  
### 8. 数组的快速定义
`List<String> numbers = Arrays.asList("1", "2", "3");`
### 9. String根据空格分割内容的正确做法
```java
String s3 = "我   是 一个         好  人啊";
String [] spString = s3.split("\\s+");
for(String ss : spString){
    System.out.println(ss);
}

我
是
一个
好
人啊
```
### 10. 获取当前时间
`long startMili=System.currentTimeMillis();`  当前时间对应的毫秒数

### 11. 定时器的使用
```java
/**
* 这个方法是调度一个task，经过2000(ms)后开始进行调度，仅仅调度一次。
*/
public static void timer1(){
Timer nTimer = new Timer();
nTimer.schedule(new TimerTask() {
    @Override
    public void run() {
    System.out.println("----设定要指定任务-----");
    }
},2000);
}

/**
* 在指定的时间点time上调度一次。
*/
public static void timer2() {
    Timer timer = new Timer();
    timer.schedule(new TimerTask() {
        public void run() {
        System.out.println("-------在指定的时间点time上调度一次。--------");
        }
    }, time);
}

/**
* 延迟5000毫秒，每1000毫秒执行一次
* 在5000（ms）后开始调度，每次调度完后，最少等待1000（ms）后才开始调度。
*/
public static void timer3() {
    Timer timer = new Timer();
    timer.scheduleAtFixedRate(new TimerTask() {
        public void run() {
        System.err.println("-------延迟5000毫秒，每1000毫秒执行一次--------");
        }
    }, 5000, 1000);
}
/**
* 设置17：56执行任务(第一次调度的时间)，每过一天执行一次
* java.util.Timer.scheduleAtFixedRate(TimerTask task, Date firstTime, long period)
*/
public static void timer4() {
    Calendar calendar = Calendar.getInstance();
    calendar.set(Calendar.HOUR_OF_DAY, 17);
    calendar.set(Calendar.MINUTE, 26);
    calendar.set(Calendar.SECOND, 0);

    Date time = calendar.getTime();

    Timer timer = new Timer();
    timer.scheduleAtFixedRate(new TimerTask() {
        public void run() {
        System.out.println("-------设定要指定任务--------");
        }
    }, time, 1000 * 60 * 60 * 24);// 这里设定将延时每天固定执行
}
```

### 12. Stack类
栈是Vector的一个子类，它实现了一个标准的后进先出的栈。   
堆栈只定义了默认构造函数，用来创建一个空栈。 堆栈除了包括由Vector定义的所有方法，也定义了自己的一些方法。   
`Stack<Integer> s1 = new Stack<Integer>()`
1. boolean empty(), 测试堆栈是否为空。
2. Object peek( ), 查看堆栈顶部的对象，但不从堆栈中移除它。
3. Object pop( ), 移除堆栈顶部的对象，并作为此函数的值返回该对象。
4. Object push(Object element), 把项压入堆栈顶部。
5. int search(Object element), 返回对象在堆栈中的位置，以 1 为基数。

### 13. char的比较
char是一个基本数据类型，只能用”==”进行值相等比较，如果想用equals()方法进行比较，则要用char的封装对Character。  
```java
//char是基本类型，没有equals()方法，只能用"=="来进行比较。
char c1 = 'a';
char c2 = 'a';
System.out.println(c1 == c2);   //true,c1和c2是同一个字符常量'a'的引用，是同一个对象地址，所以相同。

//如想用equals()方法，则可以用char的封装对象Character。下面就与String是一模一样的了，如下：
Character c3 = 'a';
Character c4 = 'a';
System.out.println(c3 == c4);  // true，c3和c4是同一个字符串常量'a'的引用，是同一个对象地址，所以相同。
System.out.println(c3.equals(c4));  //true,equals()方法只是比较两个字符串的值是否相等，地址不会进行比较,两个字符串的值都是a，所以相等。

c3 = new Character('a');
c4 = new Character('a');
System.out.println(c3 == c4);  // false，c3和c4是两个不同的对象，虽然字符串值都是a，但是对象地址不同，用==比较的话不相同。
System.out.println(c3.equals(c4));  //true，equals()方法只是比较两个字符串的值是否相等，地址不会进行比较。
```

### 14. 字符串中是否含有特殊字符
```java
//string 为要判断的字符串 
public static boolean isConSpeCharacters(String string){ 
    if(string.replaceAll("[\u4e00-\u9fa5]*[a-z]*[A-Z]*\\d*-*_*\\s*","").length()==0){ 
        //不包含特殊字符 
        return false; 
    } 
    return true; 
} 

// \u4E00-\u9FA5 匹配所有汉字
// A-Za-z0-9 匹配 带小写字母和数字
```

### 15. 移动文件到指定的目录
```java
try {
    File afile = new File("C:\\test_1\\test.txt");
    if (afile.renameTo(new File("C:\\test_2\\" + afile.getName()))) {
        System.out.println("File is moved successful!");
    } else {
        System.out.println("File is failed to move!");
    }
} catch (Exception e) {
    e.printStackTrace();
}
```