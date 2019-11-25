---
title: Eclipse插件入门
key: 2019-11-26
tags: Plug-in
---

### 1. 刷新资源
对于刷新资源关键接口是org.eclipse.core.resources.IResource，只需要调用接口的refreshLocal，比如：  
`refreshLocal(IResource.DEPTH_INFINITE, null)`  

### 2. 获取项目
```java
IWorkspaceRoot root = ResourcesPlugin.getWorkspace().getRoot();
IProject project = root.getProject(projectName);
```

----

[Eclipse插件入门-----刷新资源](https://blog.csdn.net/zyf814/article/details/8448209)

