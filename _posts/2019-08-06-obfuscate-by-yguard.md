---
title: Obfuscate by Yguard
key: 2019-08-06
tags: Java
---

### 1. code
```xml
<target depends="pack" name="obfuscate">
    <mkdir dir="${dist.dir}/obf" />
    <taskdef name="yguard" classname="com.yworks.yguard.YGuardTask">
        <classpath>
            <pathelement location="${YguardJar}" />
        </classpath>
    </taskdef>
    <yguard>
        <inoutpair in="${dist.dir}/${ant.project.name}.jar" out="${dist.dir}/obf/${ant.project.name}.jar" />
        <inoutpair in="${bin.dir}/lib/${yfiles}.jar" out="${dist.dir}/obf/${yfiles}.jar" />
        <rename logfile="rename_log.xml">
            <keep>
                <class classes="private" methods="private" fields="private">
                    <patternset>
                        <include name="com.test.ui.cdfg.custom.**" />
                    </patternset>
                </class>
            </keep>
        </rename>
    </yguard>
    <zip destfile="${dist.dir}/obf/${ant.project.name}.jar" update="true">
        <zipfileset dir="${dist.dir}/obf" includes="${yfiles}.jar" fullpath="lib/${yfiles}.jar" />
    </zip>
    <copy todir="${dist.dir}/" file="${dist.dir}/obf/${ant.project.name}.jar" />

</target>
```

### 2. 详解
此代码的背景是，需要将yfile的code进行混淆，此project引用了yfile里面的code，所以需要将project和yfile都作为输入和输出，进行混淆，但是在混淆的过程中将所有project的方法，全部都keep出去，只混淆yfile的code。   
混淆完毕后，将project中的yfile替换成混淆之后的yfile，这就zip的作用


----

[yGuard 2.7 Ant Task Documentation](https://www.yworks.com/resources/yguard/yguard_ant_howto.html)  
[YGUARD](http://www.blogjava.net/kennyr/articles/222706.html)  
[How to use Zip task](https://www.java-tips.org/other-api-tips-100035/105-ant/688-how-to-use-zip-task.html)  
