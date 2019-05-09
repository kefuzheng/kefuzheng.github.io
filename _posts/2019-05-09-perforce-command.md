---
title: Perforce基础知识
key: 2019-05-09
tags: Perforce
---

### 1. Merge
perforce merge注意事项  
必须要在要合并分支所在的工作空间，通过Depot选择History中的changelist  
在提交的时候需要CR号，注意格式CR:101010  

### 2. Commands
`rdi wall-submit -c 12311`  提交CL  
`p4 sync -f ...` p4	强制更新文件

### 3. p4在不同服务器下运行
先配置.p4config,再在workspace下运行p4 client,删除 host:xhd####，删除服务器信息；保存，退出
