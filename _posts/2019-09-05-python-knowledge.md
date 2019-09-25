---
title: Python基础知识
key: 2019-09-05
tags: Python
---

### 1. Python解释器
CPython  
这个解释器是用C语言开发的，所以叫CPython。在命令行下运行python就是启动CPython解释器。CPython是使用最广的Python解释器。教程的所有代码也都在CPython下执行。  
IPython  
IPython是基于CPython之上的一个交互式解释器，也就是说，IPython只是在交互方式上有所增强，但是执行Python代码的功能和CPython是完全一样的。  
PyPy  
PyPy是另一个Python解释器，它的目标是执行速度。PyPy采用JIT技术，对Python代码进行动态编译（注意不是解释），所以可以显著提高Python代码的执行速度。  
Jython  
Jython是运行在Java平台上的Python解释器，可以直接把Python代码编译成Java字节码执行。  
IronPython  
IronPython和Jython类似，只不过IronPython是运行在微软.Net平台上的Python解释器，可以直接把Python代码编译成.Net的字节码。
### 2. 输入和输出
`print('hello, world')` 输出
`name = input()` 让用户输入内容
### 3. 数据类型和运算符
整数、浮点数（小数）、字符串、布尔值（True、False）、空值（None）  
运算符： and， or， not  
字符串：  
转义字符\  
用r''表示''内部的字符串默认不转义  
'''字符串内容'''多行字符串  
字符串格式化：  
%运算符就是用来格式化字符串的。在字符串内部，%s表示用字符串替换，%d表示用整数替换，有几个%?占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个%?，括号可以省略  
字符串里面的%是一个普通字符怎么办？这个时候就需要转义，用%%来表示一个%  
```python
>>> 'Age: %s. Gender: %s' % (25, True)
'Age: 25. Gender: True'
```
常量： 用全部大写的变量名表示常量  
`PI = 3.14159265359`
### 4. 文件头声明
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；  
第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。
### 5. 集合类型
list列表： 有序集合，可以随时添加和删除其中的元素  
`classmates = ['Michael', 'Bob', 'Tracy']`  
tuple元组： 有序列表。tuple和list非常类似，但是tuple一旦初始化就不能修改  
`classmates = ('Michael', 'Bob', 'Tracy')`  
set: 没有重复值的list
`s = set([1, 2, 3])`  要创建一个set，需要提供一个list作为输入集合
dict字典： 键-值（key-value）存储  
```python
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}`  
d['Jack'] = 90  #存值
a = d['Jack']   #取值，但是key不存在会报错
d.get('Thomas')  #取值，key不存在，返回None
```
用len(classmates)来获得list的长度
### 6. 条件判断和循环
```python
#条件判断
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
	
#循环
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)

sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)	
```
range()函数，可以生成一个整数序列，list(range(5))
### 7. 函数
```python
#函数的定义
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x

#返回多个值，其实返回的是一个tuple
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny
#函数执行完毕也没有return语句时，自动return None。

#默认参数
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
	
enroll('Sarah', 'F')	
enroll('Bob', 'M', 7)
enroll('Adam', 'M', city='Tianjin')

#可变参数
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

>>> calc(1, 2)
5
>>> calc()
0
>>> nums = [1, 2, 3]
>>> calc(*nums)
14

#关键字参数
#关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
	
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}

#限制关键字参数的名字,*后面的参数被视为命名关键字参数。
def person(name, age, *, city, job):
    print(name, age, city, job)
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer	

#参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
	
	
```  
### 8. 高级特性
##### 1. 切片
```python
>>> L = list(range(100))
>>> L
[0, 1, 2, 3, ..., 99]
#前10个
>>> L[:10]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
#后10个
>>> L[-10:]
[90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
#前11-20个数
>>> L[10:20]
[10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
#前10个数，每两个取一个
>>> L[:10:2]
[0, 2, 4, 6, 8]

#tuple也是一种list, tuple切片
>>> (0, 1, 2, 3, 4, 5)[:3]
(0, 1, 2)

#字符串也可以看作一种list
>>> 'ABCDEFG'[:3]
'ABC'
>>> 'ABCDEFG'[::2]
'ACEG'
```
##### 2. 迭代
```pyhton
#判断一个对象是可迭代对象
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False

#enumerate函数可以把一个list变成索引-元素对
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C

#同时引用了两个变量
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print(x, y)
...
1 1
2 4
3 9
```
##### 3. 列表生成式
列表生成式时，把要生成的元素x * x放到前面，后面跟for循环，for循环后面还可以加上if判断,就可以把list创建出来  
```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]

#两层循环
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```
##### 4. 生成器(generator)
```python
#只要把一个列表生成式的[]改成()，就创建了一个generator
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>

#如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator
#最难理解的就是generator和函数的执行流程不一样。函数是顺序执行，遇到return语句或者最后一行函数语句就返回。
#而变成generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield(3)
    print('step 3')
    yield(5)
	
>>> o = odd()
>>> next(o)
step 1
1
>>> next(o)
step 2
3
>>> next(o)
step 3
5
>>> next(o)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
#odd不是普通函数，而是generator，在执行过程中，遇到yield就中断，下次又继续执行。执行3次yield后，已经没有yield可以执行了，所以，第4次调用next(o)就报错。

#斐波拉契数列（Fibonacci）
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
	
>>> for n in fib(6):
...     print(n)
...
1
1
2
3
5
8
```
##### 5. 迭代器
可以直接作用于for循环的对象统称为可迭代对象：Iterable  
凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；  
集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象。  
Python的for循环本质上就是通过不断调用next()函数实现的
### 9. 模块
一个abc.py的文件就是一个名字叫abc的模块  
通过包来组织模块，避免冲突  
每一个包目录下面都会有一个__init__.py的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。  
`__init__.py`可以是空文件，也可以有Python代码，因为__init__.py本身就是一个模块，而它的模块名就是mycompany
### 10. 面向对象编程
##### 1. 类的定义
```python
class Student(object):

	#特殊方法，类似于java中的构造函数
	#注意到__init__方法的第一个参数永远是self，表示创建的实例本身，因此，在__init__方法内部，就可以把各种属性绑定到self，因为self就指向创建的实例本身。
    def __init__(self, name, score):
        self.name = name
        self.score = score
	#方法， 数据封装
	#要定义一个方法，除了第一个参数是self外，其他和普通函数一样
	def print_score(self):
		print('%s: %s' % (std.name, std.score))

>>> bart = Student('Bart Simpson', 59)
>>> bart.name
'Bart Simpson'
>>> bart.score
59
>>> bart.print_score()
Bart Simpson: 59
```
##### 2. 访问限制
如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线__  
实例的变量名如果以__开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问  
```python
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
	
    def get_score(self):
        return self.__score
	
	def set_score(self, score):
        self.__score = score
```
变量名类似__xxx__的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以，不能用__name__、__score__这样的变量名  
以一个下划线开头的实例变量名，比如_name，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”  
双下划线开头的实例变量是不是一定不能从外部访问呢？其实也不是。不能直接访问__name是因为Python解释器对外把__name变量改成了_Student__name，所以，仍然可以通过_Student__name来访问__name变量
##### 3. 继承和多态
```python
class Animal(object):
    def run(self):
        print('Animal is running...')
		
class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')
		
def run_twice(animal):
    animal.run()
    animal.run()
	
#可以传入run_twice	
class Timer(object):
    def run(self):
        print('Start...')
```
对于静态语言（例如Java）来说，如果需要传入Animal类型，则传入的对象必须是Animal类型或者它的子类，否则，将无法调用run()方法。  
对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了  
这是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子
##### 4. 获取对象类型 
```python
#使用type() 
>>> type(123)==int
True
>>> type('abc')==str
True

#使用isinstance()
>>> isinstance(h, Husky)
True

#使用dir()
#获得一个对象的所有属性和方法
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']

#配合getattr()、setattr()以及hasattr()，可以直接操作一个对象的状态
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```
##### 5. 实例属性和类属性
```python
#给实例绑定属性的方法是通过实例变量，或者通过self变量：
#实例属性属于各个实例所有，互不干扰；
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90

#如果Student类本身需要绑定一个属性呢？可以直接在class中定义属性，这种属性是类属性，归Student类所有
#类属性属于类所有，所有实例共享一个属性；类似java中的static变量
class Student(object):
    name = 'Student'
```

----

[面向对象编程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017495723838528)  
