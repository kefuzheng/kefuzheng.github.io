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

### 3. 打开perspective
```java
IWorkbench workbench = PlatformUI.getWorkbench();
		if(workbench != null){
			IWorkbenchWindow window = workbench.getActiveWorkbenchWindow();
			if(window != null){
				try {
					workbench.showPerspective("perspective id", window);
				} catch (WorkbenchException e1) {
					e1.printStackTrace();
				}
			}
		}
```

### 4. 打开viewer
```java
//方式1
IViewPart view = PlatformUI.getWorkbench().getActiveWorkbenchWindow().getActivePage().showView("viewer id");

//方式2
IWorkbenchWindow window PlatformUI.getWorkbench().getActiveWorkbenchWindow();
IWorkbenchPage activePage = window.getActivePage();
IViewPart view = activePage.findView("viewer id");
activePage.activate(view);
```

### 5. 打开editor
```java
try {
  IDE.openEditor(window.getActivePage(), input(implements IEditorInput), "editor id");
} catch (PartInitException e) {
  e.printStackTrace();
}
```

### 6. IFile与File的转换
```java
IPath path = new Path(temp.getFilePath());
// get project
IProject project = ResourcesPlugin.getWorkspace().getRoot().getProject("project name");
String relativePath = path.toString().substring(project.getLocation().toString().length() + 1);
IFile file = project.getFile(relativePath);
```

### 7. label自动换行
```java
Label mylabel = new Label(composite, SWT.WRAP);
mylabel.setText(“xxxxxx”);
mylabel .pack();
```
要实现有换行效果的Label只需要在创建Label时，给它的Style设置为SWT.Wrap，然后对其进行pack操作即可。

### 8.Java split()方法
split() 方法根据匹配给定的正则表达式来拆分字符串。  
注意： . 、 $、 | 和 * 等转义字符，必须得加 \\。  
注意：多个分隔符，可以用 | 作为连字符。  

### 9.重绘监听
```java
Composite textComp = new Composite(pragmaComposite, SWT.NONE);
textComp.setLayout(new GridLayout(1, false));
textGrid = new GridData(SWT.FILL, SWT.FILL, false, true, 1, 1);
textGrid.heightHint = 40;
textGrid.exclude = true;
		
textComp.setLayoutData(textGrid);
textComp.addPaintListener(new PaintListener() {
	@Override
	public void paintControl(PaintEvent e) {
		GC gc = e.gc;
		gc.setForeground(Display.getDefault().getSystemColor(SWT.COLOR_RED));
		gc.drawText("RESOURCE directive is deprecated, use\nBIND_OP or BIND_STORAGE instead",
				5, 0);
	}
});
```
对composite进行重绘，重新写了文字，这种方法可以用在，有主题颜色覆盖，一般常用的改字体颜色不生效的情况下

### 10.对editor中添加、删除marker
```java
import org.eclipse.core.resources.IMarker;

private void createWarningMarker(String lineNumber, String errorMessage,
		IFile currentFile) {
	IMarker marker;
	try {
		if (currentFile == null || !currentFile.exists()) {
			return;
		}
		marker = currentFile.createMarker(IMarker.PROBLEM);
		if (marker.exists()) {
			marker.setAttribute(IMarker.MESSAGE, errorMessage);
			marker.setAttribute(IMarker.LINE_NUMBER,
					Integer.parseInt(lineNumber));
			marker.setAttribute(IMarker.SEVERITY, IMarker.SEVERITY_WARNING);
			marker.setAttribute(IMarker.LOCATION,
					Integer.parseInt(lineNumber));
		}
	} catch (CoreException e) {
		e.printStackTrace();
	}
}

private void removeWarningMarker(List<Integer> errorLines, IFile currentFile) {
	try {
		IMarker[] markers = currentFile.findMarkers(IMarker.PROBLEM, false, 0);
		for(IMarker marker : markers) {
			if(!errorLines.contains(marker.getAttribute(IMarker.LINE_NUMBER, -5))) {
				marker.delete();
			}
		}
	} catch (CoreException e) {
		e.printStackTrace();
	}
}
```

----

[Eclipse插件入门-----刷新资源](https://blog.csdn.net/zyf814/article/details/8448209)

