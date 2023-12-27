---
title: Clangd-query的使用
key: 2023-12-26
tags: clang
---

### 1. 什么是clangd-query
clang-query是一个基于libclang库的命令行界面工具，可以在clang抽象语法树的上下文执行查询，帮助用户理解代码的结构

### 2. clang-query的原理
使用clang-query先通过clang编译源代码，产生一个编译数据库，然后再向clang-query提供这个数据库供以查询

### 3. clang-query的功能
clang-query可以执行某个表达式的匹配，显示抽象语法树上的节点，设置和获取变量等，支持动态模糊匹配，大大提高了查询的灵活性和匹配的准确性

##### 1. ASTMatcher
ASTMatcher：允许用户编写一个程序来匹配AST节点并能通过访问节点的c++接口来获取该AST节点的属性、源位置等任何信息，其主要由宏与模板驱动，用法和函数式编程类似，其可实现简单精准高效的匹配。   
主要分为三类：   
- Note Matchers：匹配特定类型节点，eg. objcPropertyDecl() ：匹配OC属性声明节点
- Narrowing Matchers：匹配具有相应属性的节点，eg.hasName()、hasAttr()：匹配具有指定名称、attribute的节点
- AST Traversal Matchers：允许在节点之间递归匹配，eg.hasAncestor()、hasDescendant()：匹配祖、后代类节点

### 4. 常用配置

- set output detailed-ast: 输出格式为详细的AST
- set traversal IgnoreUnlessSpelledInSource: 设置遍历模式，只处理在源代码中显示出现的节点，忽略由编译器隐式生成的、并非直接在源代码中的节点
- set bind-root true: 把整个匹配的AST绑定到根节点
- set print-matcher true: 在打印匹配的节点时，一起打印匹配它的匹配器
- enable output dump: 启用dump输出模式，输出匹配节点的详细信息

### 5. 常用命令
```shell
# 完整命令
clang-query -c "set output dump" -c "enable output print" -c "match parmVarDecl(isExpansionInMainFile(), hasAncestor(functionDecl(hasName("testBlackboxFunc"))), hasName("in"))" t.cpp -extra-arg -Iinclude -extra-arg -Iinclude/etc

# 当前文件的function declaration list
functionDecl(isExpansionInMainFile(), unless(isInline()), unless(isConstexpr()))

# 查询当前文件中的某个function
functionDecl(hasName("%s"), isExpansionInMainFile(), unless(isInline()), unless(isConstexpr()))

# 查询指定function中的指定parameter
parmVarDecl(isExpansionInMainFile(), hasAncestor(functionDecl(hasName("testBlackboxFunc"))), hasName("in"))

```

----

[Examining the Clang AST with clang-query](https://devblogs.microsoft.com/cppblog/exploring-clang-tooling-part-2-examining-the-clang-ast-with-clang-query/)  
[AST Matcher Reference](https://clang.llvm.org/docs/LibASTMatchersReference.html)   
[ASTMatcher分析函数调用链](https://cloud.tencent.com/developer/article/1523137)   
[clang-query使用](https://blog.csdn.net/cppclub/article/details/134233861)   
