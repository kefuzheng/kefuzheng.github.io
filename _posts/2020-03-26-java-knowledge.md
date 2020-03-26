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
