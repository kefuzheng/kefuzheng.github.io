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

