---
title: Python常见问题
key: 2019-11-29
tags: Python
---

### 1. list, set, OrderedDict的使用
```python
#  list
len([1, 2, 3])  #list长度
[1, 2, 3] + [4, 5, 6]	#list组合
3 in [1, 2, 3]  #元素是否在列表中
list.append(obj)  #在列表末尾添加新的对象
''.jion([a,d,c,d])  #list转为string
# for 循环获取list的index索引
for inx, val in enumerate(['uyy', 'dfdf']):
	print(inx)
	print(val)
	
#  set的定义
s = set()
s = set([1,1,2,3])  # list 转set

#  有序字典的定义
import collections
d1=collections.OrderedDict()
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
### 5. json模块
loads()：将json数据转化成dict数据
dumps()：将dict数据转化成json数据
load()：读取json文件数据，转成dict数据
dump()：将dict数据转化成json数据后写入json文件
```python
import json

# json字符串转成dict字典
dict = json.loads(j)

# dict字典转json字符串
json_data = json.dumps(dict)

# 读取json文件数据，转成dict数据
with open('1.json', 'r') as f:
    dict = json.load(f)

# 将dict数据转化成json数据后写入json文件
with open('1.json', 'w') as f:
    json.dump(dict, f)
    
# 排序并且缩进两个字符输出
json.dumps(data, sort_keys=True, indent=2)
```
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
### 9. 读取excel表格
```python
import xlrd

def parse_xlsx(path):
    workbook = xlrd.open_workbook(path)
    table = workbook.sheet_by_name('Sheet1')  # 获得表格
    total_rows = table.nrows  # 拿到总共行数
    columns = table.row_values(0)  # 获取表头 ['姓名', '用户名', '联系方式', '密码']
    excel_list = []
    for one_row in range(1, total_rows):  # 也就是从Excel第二行开始，第一行表头不算
        row = table.row_values(one_row)
        if row:
            row_object = {}
            for i in range(0, len(columns)):
                key = columns[i]
                row_object[key] = row[i]  # 表头与数据对应
            excel_list.append(row_object)
    return excel_list
```
### 10. strip()的用法
`'   ####test  '.strip()'` #去除空格换行符  
`'####test'.strip('#')'` # 去除字符#
### 11. os模块
```python
os.getcwd() #用以获取当前的工作目录  
os.chdir(path) #用于改变当前工作目录到指定的路径  
os.chmod(file_name, stat.S_IRWXU | stat.S_IRWXO | stat.S_IRWXG)  #更改权限  
os.path.exists(file)  #判断文件是否存在
os.path.getsize(file)  #判断文件是否为空
os.mkdir(folder)  #创建单个目录
```
### 12. logging模块
将关键信息输出到console，将所有的信息输入到log文件
```
import logging as log

log.basicConfig(level=log.DEBUG, filename='test.log', format='%(levelname)-8s %(message)s', filemode='w')
console = log.StreamHandler()
console.setLevel(log.INFO)
formatter = log.Formatter('[%(levelname)s] %(message)s')
console.setFormatter(formatter)
log.getLogger('').addHandler(console)
#  只输入到log
log.debug('test debug')
#  log和console
log.info('test info')
```

----

[Python基础知识](https://kefuzheng.github.io/2019/09/05/python-knowledge.html)  
[Python进阶](https://kefuzheng.github.io/2019/10/10/python-advanced-knowledge.html)   
[Python + logging 输出到屏幕，将log日志写入文件](https://www.cnblogs.com/nancyzhu/p/8551506.html)  
[python logging模块使用教程](https://www.jianshu.com/p/feb86c06c4f4)
