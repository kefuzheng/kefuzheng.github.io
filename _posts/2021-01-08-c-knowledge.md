---
title: C/C++基础知识
key: 2020-01-08
tags: C/C++
---

### 1. GDB调试
##### 1. test code
```c
#include <stdio.h>
int main ()
{
   unsigned long long int n, sum;
   n = 1;
   sum = 0;

   while (n <= 100)
   {
      sum = sum + n;
      n = n + 1;
   }
   return 0;
}
```
##### 2. compile
需要使用 gcc -g 选项编译源文件，即可生成满足 GDB 要求的可执行文件。
```shell
[root demo]# ls
main.c
[root demo]# gcc main.c -o main.exe -g
[root demo]# ls
main.c  main.exe
```
GCC 编译器支持 -O（等于同 -O1，优化生成的目标文件）和 -g 一起参与编译,对 GDB 调试器更友好的是 -Og 选项，-Og 对代码所做的优化程序介于 O0 ~ O1 之间，真正可做到“在保持快速编译和良好调试体验的同时，提供较为合理的优化级别”
##### 3. 启动GDB调试器
```shell
[root demo]# gdb main.exe
GNU gdb (GDB) 8.0.1
Copyright (C) 2017 Free Software Foundation, Inc.
......
(gdb) 
```
注意，该指令在启动 GDB 的同时，会打印出一堆免责条款。通过添加 --silent（或者 -q、--quiet）选项，可将比部分信息屏蔽掉
##### 4. 常用命令
- b/break xxx：在源代码指定的某一行设置断点，其中 xxx 用于指定具体打断点的位置。
- r/run：执行被调试的程序，其会自动在第一个断点处暂停执行。
- c/continue：当程序在某一断点处停止运行后，使用该指令可以继续执行，直至遇到下一个断点或者程序结束。
- n/next：令程序一行代码一行代码的执行。
- p/print xxx：打印指定变量的值，其中 xxx 指的就是某一变量名
- q/quit：终止调试。

### 2. GDB调试多线程程序
##### 1. test code
```c
#include <stdio.h>
#include <pthread.h>
void* thread_job(void*name)
{
    char * thread_name = (char*)name;
    printf("this is %s\n",thread_name);
    printf("test mul-thread\n");
}
int main()
{
    pthread_t tid1,tid2;
    pthread_create(&tid1, NULL, thread_job, "thread1_job");
    pthread_create(&tid2, NULL, thread_job, "thread2_job");
    pthread_join(tid1,NULL);
    pthread_join(tid2,NULL);
    printf("this is main\n");
    return 0;
}
```
##### 2. compile
`gcc main.c -o main.exe -g -lpthread`   
因为 pthread 线程库并不属于 Linux 系统中的默认库，所以编译、链接时就需要为 gcc 命令附加 -lpthread 参数。
##### 3. 常用命令
- info threads: 查看当前调试环境中包含多少个线程，并打印出各个线程的相关信息，包括线程编号（ID）、线程名称等。
- thread id: 将线程编号为 id 的线程设置为当前线程。
- thread apply id... command: id... 表示线程的编号；command 代指 GDB 命令，如 next、continue 等。整个命令的功能是将 command 命令作用于指定编号的线程。当然，如果想将 command 命令作用于所有线程，id... 可以用 all 代替
- break location thread id: 在 location 指定的位置建立普通断点，并且该断点仅用于暂停编号为 id 的线程。
- set scheduler-locking off|on|step: 默认情况下，当程序中某一线程暂停执行时，所有执行的线程都会暂停；同样，当执行 continue 命令时，默认所有暂停的程序都会继续执行。该命令可以打破此默认设置，即只继续执行当前线程，其它线程仍停止执行。

##### 4. 设置线程锁
默认情况下，无论哪个线程暂停执行，其它线程都会随即暂停；反之，一旦某个线程启动（借助 next、step、continue 命令），其它线程也随即启动。GDB 调试默认的这种调试模式（称为全停止模式），一定程序上可以帮助我们更好地监控程序中各个线程的执行。注意，当对某个线程进行单步调试时，其它线程也会随即执行和停止，但执行的往往不只是一行代码，可能是多行代码。  
一些场景中，我们可能只想让某一特定线程运行，其它线程仍维持暂停状态。要想达到这样的效果，就需要借助 set scheduler-locking 命令。 此命令可以帮我们将其它线程都“锁起来”，使后续执行的命令只对当前线程或者指定线程有效，而对其它线程无效。   
`(gdb) set scheduler-locking mode`   
其中，参数 mode 的值有 3 个，分别为 off、on 和 step，它们的含义分别是：
- off：不锁定线程，任何线程都可以随时执行；
- on：锁定线程，只有当前线程或指定线程可以运行；
- step：当单步执行某一线程时，其它线程不会执行，同时保证在调试过程中当前线程不会发生改变。但如果该模式下执行 continue、until、finish 命令，则其它线程也会执行，并且如果某一线程执行过程遇到断点，则 GDB 调试器会将该线程作为当前线程。
  
### 3. c++ sleep
```c++
// windows OS
#include <iostream>
#include <windows.h>
using namespace std;
void main()
{
    //睡眠5秒再输出
    Sleep(5000);
    std::cout << "Hi,Gril!" << std::endl;
}

// linux OS
include <unistd.h>
using namespace std;
int main()
{
	//5秒后输出Hi,Gril!
	sleep(5);
	std::cout << "Hi,Gril!" << std::endl;

	//3000000微妙(相当于3秒)输出Hi,Boy!
	usleep(3000000);
	std::cout << "Hi,Boy!" << std::endl;

	return 0;
}
```
 
----

[GDB调试C/C++程序](http://c.biancheng.net/view/8153.html)  
[GDB调试多线程程序](http://c.biancheng.net/view/8261.html)   

