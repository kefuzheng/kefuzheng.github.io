---
title: Vim基础知识
key: 2019-08-09
tags: Linux
---

### 1. 查找（/）
在normal模式下按下 / 即可进入查找模式，输入要查找的字符串并按下回车。 Vim会跳转到第一个匹配。  
n 查找下一个，N 查找上一个。  
Vim查找支持正则表达式，例如/vim$匹配行尾的"vim"。 需要查找特殊字符需要转义，例如/vim\$匹配"vim$"。
### 2. 大小写敏感查找(\c)
在查找模式中加入 \c 表示大小写不敏感查找， \C 表示大小写敏感查找。例如：  
`/foo\c`  
将会查找所有的"foo","FOO","Foo"等字符串。
### 3. 大小写敏感配置
Vim 默认采用大小写敏感的查找，为了方便我们常常将其配置为大小写不敏感：  
```
" 设置默认进行大小写不敏感查找
set ignorecase
" 如果有一个大写字母，则切换到大小写敏感查找
set smartcase 
```
将上述设置粘贴到你的~/.vimrc，重新打开Vim即可生效
### 4. 查找当前单词(*)
在normal模式下按下 * 即可查找光标所在单词（word），要求每次出现的前后为空白字符或标点符号。  
例如当前为foo，可以匹配foo bar中的foo，但不可匹配foobar中的foo。 这在查找函数名、变量名时非常有用。  
按下 g* 即可查找光标所在单词的字符序列，每次出现前后字符无要求。 即foo bar和foobar中的foo均可被匹配到
### 5. 查找与替换(:s)
:s（substitute）命令用来查找和替换字符串。语法如下：  
`:{作用范围}s/{目标}/{替换}/{替换标志}`  
例如:%s/foo/bar/g会在全局范围(%)查找foo并替换为bar，所有出现都会被替换（g）
### 6. 作用范围
作用范围分为当前行、全文、选区等等。  
`:s/foo/bar/g` 当前行
`:%s/foo/bar/g` 全文
`:'<,'>s/foo/bar/g` 选区，在Visual模式下选择区域后输入:，Vim即可自动补全为 :'<,'>。
`:5,12s/foo/bar/g` 2-11行
`:.,+2s/foo/bar/g` 当前行.与接下来两行+2：
`:n,$s/vivian/sky/` 替换第n行开始到最后一行中每一行的第一个vivian为sky 
`:n,$s/vivian/sky/g` 替换第n行开始到最后一行中每一行所有vivian为sky 
`:s#vivian/#sky/#` 替换当前行第一个vivian/为sky/  　可以使用 # 作为分隔符，此时中间出现的 / 不会作为分隔符 
### 7. 删除文本中的^M
问题描述：对于换行，window下用回车换行（0A0D）来表示，linux下是回车（0A）来表示。这样，将window上的文件拷到unix上用时，总会有个^M.请写个用在unix下的过滤windows文件的换行符（0D）的shell或c程序。  　 
`:cat filename1 | tr -d “^V^M” > newfile`  
`:sed -e “s/^V^M//” filename > outputfilename`  
需要注意的是在1、2两种方法中，^V和^M指的是Ctrl+V和Ctrl+M.你必须要手工进行输入，而不是粘贴。 
### 8. 在Vim中设置Tab
```
set tabstop=4  #表示一个tab显示出来是多少个空格的长度，默认 8。
set softtabstop=4  #Tab 转为多少个空格。
set shiftwidth=4   #在文本上按下>>（增加一级缩进）、<<（取消一级缩进）或者==（取消全部缩进）时，每一级的字符数。
set expandtab   由于 Tab 键在不同的编辑器缩进不一致，该设置自动将 Tab 转为空格。
```
### 9. Vim禁止生成临时文件
打开.vimrc，找到:set backup 注释掉
（Vim的注释符是"）
即修改后应该是这样的：
```
if has("vms")
set nobackup " do not keep a backup file, use versions instead
" else
" set backup " keep a backup file
```
### 10. 不能右键粘贴设置方法
编辑 ~/.vimrc 文件，加入如下代码： 
```
if has('mouse') 
	set mouse-=a endif 
```
### 11. vim的推荐配置



----

[vim查找/替换字符串](https://www.cnblogs.com/GODYCA/archive/2013/02/22/2922840.html)  
[关于在vim中的查找和替换](https://www.cnblogs.com/huxinga/p/7942194.html)  
