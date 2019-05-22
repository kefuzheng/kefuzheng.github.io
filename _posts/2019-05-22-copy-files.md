---
title: java复制文件的4中方式
key: 2019-05-22
tags: Java
---

尽管Java提供了一个可以处理文件的IO操作类。 但是没有一个复制文件的方法。
### 1. FileStreams
这是最经典的方式将一个文件的内容复制到另一个文件中。使用FileInputStream读取文件A的字节，使用FileOutputStream写入到文件B
```java
private static void copyFileUsingFileStreams(File source, File dest)
        throws IOException {    
    InputStream input = null;    
    OutputStream output = null;    
    try {
           input = new FileInputStream(source);
           output = new FileOutputStream(dest);        
           byte[] buf = new byte[1024];        
           int bytesRead;        
           while ((bytesRead = input.read(buf)) > 0) {
               output.write(buf, 0, bytesRead);
           }
    } finally {
        input.close();
        output.close();
    }
}
```
执行几个读和写操作try的数据,所以这应该是一个低效率的
### 2. FileChannel
Java NIO包括transferFrom方法,根据文档应该比文件流复制的速度更快
```java
private static void copyFileUsingFileChannels(File source, File dest) throws IOException {    
        FileChannel inputChannel = null;    
        FileChannel outputChannel = null;    
    try {
        inputChannel = new FileInputStream(source).getChannel();
        outputChannel = new FileOutputStream(dest).getChannel();
        outputChannel.transferFrom(inputChannel, 0, inputChannel.size());
    } finally {
        inputChannel.close();
        outputChannel.close();
    }
}
```
拷贝大文件速度最快
### 3. Commons IO
Apache Commons IO提供拷贝文件方法在其FileUtils类,可用于复制一个文件到另一个地方。它非常方便使用Apache Commons FileUtils类时。  
基本上,这个类使用Java NIO FileChannel内部
扩展点中变量名称（%Name）的使用  
需要将其变量定义在plugin.properties中，然后文件需要在MANIFEST.MF中注册  
```java
private static void copyFileUsingApacheCommonsIO(File source, File dest)
        throws IOException {
    FileUtils.copyFile(source, dest);
}
```
### 4. Java7的Files类
```java
private static void copyFileUsingJava7Files(File source, File dest)
        throws IOException {    
    Files.copy(source.toPath(), dest.toPath());
}
```
