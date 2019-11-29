---
title: python常见问题
key: 2019-11-29
tags: Python
---

### 1. list的使用
```python
len([1, 2, 3])  #list长度
[1, 2, 3] + [4, 5, 6]	#list组合
3 in [1, 2, 3]  #元素是否在列表中
list.append(obj)  #在列表末尾添加新的对象
''.jion([a,d,c,d])  #list转为string
# for 循环获取list的index索引
for inx, val in enumerate(['uyy', 'dfdf']):
	print(inx)
	print(val)
```
### 2. 定义全局变量的用法
##### 1. 声明法
该方法是直接在当前模块中进行全局变量定义声明，使用global的声明方式，再进行引用！  
```python
OLD_URL='https://kefuzheng.github.io/'

def bb():
    global OLD_URL
    OLD_URL = OLD_URL +'#m'
 
if __name__=='__main__':
    bb()
    print OLD_URL
#输出：
https://kefuzheng.github.io/#m
```
##### 2. 模块法
把全局变量定义在一个单独模块中，然后在需要使用的全局模块中将定义的全局变量模块导入  
```python

#gl.py  全局变量模块定义
GL_A='hello'
GL_B='world'

#test.py 全局变量引用模块
import gl

print gl.GL_A， gl.GL_B
```
### 3. python3下虚拟环境的创建和使用
`python3 -m venv /path/to/myenv`  
这条命令将创建/path/to/myenv目录，并在其中创建包含Python解释器、标准库和各种支持  
`source /path/to/myenv/bin/activate` 激活并使用它  
### 4. python中执行shell命令
##### 1. system()
其中最后一个0是这个命令的返回值，为0表示命令执行成功。使用system无法将执行的结果保存起来。
```python
import os
os.system('ls')
```
##### 2. popen()
获取命令执行的结果，但是没有命令的执行状态，这样可以将获取的结果保存起来放到list或string中。
```python
import os
os.popen('ls').read()  # readlines()
```
##### 3. 调用shell脚本
```python
import os
os.system('./hello.sh')
```
### 5. 格式化输出json数据
`json.dumps(data, sort_keys=True, indent=2)` # 排序并且缩进两个字符输出
### 6. 读取xml文件
```python
try:
    import xml.etree.cElementTree as ET
except ImportError:
    import xml.etree.ElementTree as ET

try:
	tree = ET.parse(xml_path)
	root = tree.getroot()
except Exception as e:
	print(e)
	sys.exit(1)

for msg in root.findall('Message'):
	key = msg.get('key')
	id_prefix = msg.get('idPrefix')
	id_num = msg.get('idNum')
	tcl_message = msg.get('tclMessage')
	print('key: %s; prefix: %s' % (key, id_prefix)
```
### 7. 读取目录下的所有子目录及其文件
```python
def read_dir(file_path):
    all_dir = os.listdir(file_path)
    for each_dir in all_dir:
        child = os.path.join(file_path, each_dir)
        if os.path.isfile(child):
            read_file(child)
        if os.path.isdir(child):
            read_dir(child)
			
def read_file(file)
	print('This is file')
```
### 8. 生成excel表格
```python
import xlsxwriter as xlsx

workbook = xlsx.Workbook(file_name)
worksheet = workbook.add_worksheet('sheet_name')  #可什么不写，呈现默认值
worksheet.set_column('A:A', 15)   #第一列宽度
worksheet.set_column('B:B', 20)
worksheet.set_column('C:C', 20)
worksheet.set_column('D:D', 6)
worksheet.set_column('E:E', 55)
worksheet.set_column('F:F', 20)
worksheet.set_column('G:G', 40)
bold = workbook.add_format({'bold': True})   #定义字体
# 用第一种方式，设置内容
worksheet.write('A1', 'ID', bold)    
worksheet.write('B1', 'OWNERS', bold)
worksheet.write('C1', 'KEY', bold)
worksheet.write('D1', 'LINE', bold)
worksheet.write('E1', 'FILE', bold)
worksheet.write('F1', 'CONTENT', bold)
worksheet.write('G1', 'REFERENCE', bold)
# 用第二种方式，设置内容（行的开始是1，列的开始是0）
worksheet.write(2, 0, 'test1') # 等于A2
worksheet.write(2, 1, 'test2')
workbook.close()
```
### 9. strip()的用法
`'   ####test  '.strip()'` #去除空格换行符
`'####test'.strip('#')'` # 去除字符#

----

[Python基础知识](https://kefuzheng.github.io/2019/09/05/python-knowledge.html)  
[Python进阶](https://kefuzheng.github.io/2019/10/10/python-advanced-knowledge.html)   
