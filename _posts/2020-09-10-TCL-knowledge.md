---
title: TCL基础知识
key: 2020-09-10
tags: TCL
---

### 1. TCL中用于文件处理的命令
- file atime name返回文件的最近存取时间。
- file dirname name返回name所描述的文件名的目录部分。eg：source [file dirname $argv0]/vxTestCommon.tcl
- file executable name返回文件是否可被执行。
- file exists name返回1 表示文件存在，0 表示文件不存在。
- file extension name返回文件的扩展名。
- file isdirectory name判断是否为目录。
- file isfile name判断是否为文件。
- file lstat name varName以数组形式返回。执行lstat系统函数。存储在varName。
- file mtime name文件的最近修改时间。
- file owned name判断文件是否属于你。
- file readable name判断文件是否可读。
- file readlink name都出符号连接的真正的文件名。
- file rootname name返回不包括最后一个点的字符串。
- file size name返回文件的大小。
- file stat name varName调用stat内核调用，以数组形式存在varName中。
- file tail name返回最后一个斜线以后的部分。
- file type name返回文件类型file, directory, characterSpecial, blockSpecial, fifo, link, 或socket。
- file writable name判断文件是否可写。
- flush fileId立即处理由fileId描述的文件缓冲区。
