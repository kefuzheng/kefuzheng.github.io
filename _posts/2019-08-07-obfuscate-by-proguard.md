---
title: Obfuscate by ProGuard
key: 2019-08-07
tags: Java
---

### 1. ProGuard简介
因为Java代码是非常容易反编码的，为了很好的保护Java源代码，我们需要对编译好后的class文件进行混淆。  
ProGuard是一个混淆代码的开源项目，它的主要作用是混淆代码，殊不知ProGuard还包括以下4个功能。  
压缩(Shrink)：检测并移除代码中无用的类、字段、方法和特性（Attribute）。  
优化(Optimize)：对字节码进行优化，移除无用的指令。  
混淆(Obfuscate)：使用a，b，c，d这样简短而无意义的名称，对类、字段和方法进行重命名。  
预检(Preveirfy)：在Java平台上对处理后的代码进行预检，确保加载的class文件是可执行的。  
总而言之，根据官网的翻译：Proguard是一个Java类文件压缩器、优化器、混淆器、预校验器。压缩环节会检测以及移除没有用到的类、字段、方法以及属性。优化环节会分析以及优化方法的字节码。混淆环节会用无意义的短变量去重命名类、变量、方法。这些步骤让代码更精简，更高效，也更难被逆向（破解）。
### 2. ProGuard用法
##### 1. 命令行调用
`java -jar proguard.jar options ...`  
`java -jar proguard.jar @proguard.cfg`   
`java -jar proguard.jar @proguard.cfg -verbose`  
把proguard的参数写到一个配置文件中，比如说proguard.cfg，也可以混合使用
##### 2. ant中应用Proguard
```xml
<target depends="pack" name="obfuscate">
        <taskdef resource="proguard/ant/task.properties"
            classpath="lib/proguard.jar" />
	<!-- 若报错，尝试classpath完整路径	
	<taskdef resource="proguard/ant/task.properties">
		<classpath>
		    <pathelement location="${ProGuardRoot}/lib/proguard.jar" />
		</classpath>
  	</taskdef>
	-->
	<proguard configuration="proguard.pro">
		-injars ${dist.dir}/${ant.project.name}.jar
		-outjar ${dist.dir}/../${ant.project.name}.jar
		-libraryjars ${ECLIPSE_HOME}/plugins/org.eclipse.ui_3.109.0.v20170411-1742.jar
	</proguard>
</target>
```		
##### 3. 参数的配置
```
# 需要读取的配置文件,-include 的简写
@filename 

# 需要读取的配置文件
-include filename 

# 为所有引用的相对路径指定一个根路径
-basedirectory directory 

# 指定输入的包，可以包括 jar, aar, war, ear, zip, apk或者文件目录。
# 这些包或者目录下的class文件将被处理后写入到输出文件中。默认情况下非class文件会被原封不动的复制到输出文件中。
-injars classpath 

# 指定输出文件，类型包括 jar, aar, war, ear, zip, apk和 目录。
-outjars classpath 

# 指定输入文件引用的类库。这些类库不会被写入到输出文件中。每个库至少要有一个类被引用。
-libraryjars classpaath 

# 代码混淆压缩比，在0和7之间，默认为5，一般不需要改
-optimizationpasses 5
 
# 混淆时不使用大小写混合，混淆后的类名为小写
-dontusemixedcaseclassnames
 
# 指定不去忽略非公共的库的类
-dontskipnonpubliclibraryclasses
 
# 指定不去忽略非公共的库的类的成员
-dontskipnonpubliclibraryclassmembers
 
# 不做预校验，preverify是proguard的4个步骤之一
# Android不需要preverify，去掉这一步可加快混淆速度
-dontpreverify
 
# 有了verbose这句话，混淆后就会生成映射文件
# 包含有类名->混淆后类名的映射关系
# 然后使用printmapping指定映射文件的名称
-verbose
-printmapping proguardMapping.txt
 
# 指定混淆时采用的算法，后面的参数是一个过滤器
# 这个过滤器是谷歌推荐的算法，一般不改变
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*
 
# 保护代码中的Annotation不被混淆，这在JSON实体映射时非常重要，比如fastJson
-keepattributes *Annotation*
 
# 避免混淆泛型，这在JSON实体映射时非常重要，比如fastJson
-keepattributes Signature
 
# 抛出异常时保留代码行号，在异常分析中可以方便定位
-keepattributes SourceFile,LineNumberTable
# 用于告诉ProGuard，不要跳过对非公开类的处理。
# 默认情况下是跳过的，因为程序中不会引用它们，有些情况下人们编写的代码与类库中的类在同一个包下，
# 并且对包中内容加以引用，此时需要加入此条声明。
-dontskipnonpubliclibraryclasses

# 这个是给Microsoft Windows用户的，因为ProGuard假定使用的操作系统是能区分两个只是大小写不同的文件名，
# 但是Microsoft Windows不是这样的操作系统，所以必须为ProGuard指定-dontusemixedcaseclassnames选项
-dontusemixedcaseclassnames

# 保留所有的本地native方法不被混淆
-keepclasseswithmembernames class * {
    native <methods>;
}
 
# 保留了继承自Activity、Application这些类的子类
# 因为这些子类，都有可能被外部调用
# 比如说，第一行就保证了所有Activity的子类不要被混淆
-keep public class * extends android.app.Activity
-keep public class * extends android.app.Application
-keep public class * extends android.app.Service
-keep public class * extends android.content.BroadcastReceiver
-keep public class * extends android.content.ContentProvider
-keep public class * extends android.app.backup.BackupAgentHelper
-keep public class * extends android.preference.Preference
-keep public class * extends android.view.View
-keep public class com.android.vending.licensing.ILicensingService
 
# 如果有引用android-support-v4.jar包，可以添加下面这行
-keep public class com.xxxx.app.ui.fragment.** {*;}
 
# 保留在Activity中的方法参数是view的方法，
# 从而我们在layout里面编写onClick就不会被影响
-keepclassmembers class * extends android.app.Activity {
    public void *(android.view.View);
}
 
# 枚举类不能被混淆
-keepclassmembers enum * {
public static **[] values();
public static ** valueOf(java.lang.String);
}
 
# 保留自定义控件（继承自View）不被混淆
-keep public class * extends android.view.View {
    *** get*();
    void set*(***);
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
}
 
# 保留Parcelable序列化的类不被混淆
-keep class * implements android.os.Parcelable {
    public static final android.os.Parcelable$Creator *;
}
 
# 保留Serializable序列化的类不被混淆
-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}
 
# 对于R（资源）下的所有类及其方法，都不能被混淆
-keep class **.R$* {
    *;
}
 
# 对于带有回调函数onXXEvent的，不能被混淆
-keepclassmembers class * {
    void *(**On*Event);
}
```
### 3. 示例applications.pro
```
#
# This ProGuard configuration file illustrates how to process applications.
# Usage:
#     java -jar proguard.jar @applications.pro
#

-verbose

# Specify the input jars, output jars, and library jars.

-injars  in.jar
-outjars out.jar

# Before Java 9, the runtime classes were packaged in a single jar file.
#-libraryjars <java.home>/lib/rt.jar

# As of Java 9, the runtime classes are packaged in modular jmod files.
-libraryjars <java.home>/jmods/java.base.jmod(!**.jar;!module-info.class)
#-libraryjars <java.home>/jmods/.....

#-libraryjars junit.jar
#-libraryjars servlet.jar
#-libraryjars jai_core.jar
#...

# Save the obfuscation mapping to a file, so you can de-obfuscate any stack
# traces later on. Keep a fixed source file attribute and all line number
# tables to get line numbers in the stack traces.
# You can comment this out if you're not interested in stack traces.

-printmapping out.map
-renamesourcefileattribute SourceFile
-keepattributes SourceFile,LineNumberTable

# Preserve all annotations.

-keepattributes *Annotation*

# You can print out the seeds that are matching the keep options below.

#-printseeds out.seeds

# Preserve all public applications.

-keepclasseswithmembers public class * {
    public static void main(java.lang.String[]);
}

# Preserve all native method names and the names of their classes.

-keepclasseswithmembernames,includedescriptorclasses class * {
    native <methods>;
}

# Preserve the special static methods that are required in all enumeration
# classes.

-keepclassmembers,allowoptimization enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

# Explicitly preserve all serialization members. The Serializable interface
# is only a marker interface, so it wouldn't save them.
# You can comment this out if your application doesn't use serialization.
# If your code contains serializable classes that have to be backward 
# compatible, please refer to the manual.

-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    static final java.io.ObjectStreamField[] serialPersistentFields;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}

# Your application may contain more items that need to be preserved; 
# typically classes that are dynamically created using Class.forName:

# -keep public class com.example.MyClass
# -keep public interface com.example.MyInterface
# -keep public class * implements com.example.MyInterface
```
### 4. Notes
1. jdk8和jdk9以上对于jdk基本库的依赖的不同；  
2. 在插件开发中，在扩展点中扩展的viewer和editor等，将具体的class绑定在扩展点中的，需要keep出去；  
3. 反射调用，需要keep出去。
4. 若是想将import的第三方的class也进行混淆，需要将这个第三方的jar包放到injars，这样就能混淆它的所有引用，而不影响runtime。

----

[Android Proguard(混淆)](https://www.jianshu.com/p/60e82aafcfd0)  
[ProGuard代码混淆技术详解](https://www.cnblogs.com/cr330326/p/5534915.html)  
[ProGuard in Ant](https://www.guardsquare.com/en/products/proguard/manual/ant)  
