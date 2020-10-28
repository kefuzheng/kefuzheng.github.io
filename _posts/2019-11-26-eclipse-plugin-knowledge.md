---
title: Eclipse插件知识点
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
### 11. SWT的Text的几种监听事件
1. 文本框按键监听事件，遇到的应用场景：需要对文本框进行输入长度限制时
```java
txtSample.addKeyListener(new KeyAdapter() { 　//按键监听
    @Override
    public void keyPressed(KeyEvent e) {
        if(txtSample.getText().length > 3){ //判断文本框输入长度是否大于4
            e.doit = false;  //doit属性如果为true,则字符允许输入,反之不允许 
        }
    }
});
```
2. 文本框焦点获取监听，遇到的应用场景：当文本框的输入不是使用键盘而是界面上的按钮键盘。
```java
countText1.addFocusListener(new FocusAdapter() { // 焦点监听
    @Override
    public void focusGained(FocusEvent arg0) {
        flag = 1;  //flag为一个全局变量，用来标志当前是在哪个文本框进行输入
        }
});
```
3. 内容改变监听事件，遇到的应用场景：一个文本框内容改变时，其他文本框或label标签需要同步改变
```java
countText1.addModifyListener(new ModifyListener() {// 内容改变监听
     @Override
     public void modifyText(ModifyEvent arg0) {
         setTotalValue(); //用来进行计算总计项的方法
         }
});
```
4. 输入类型限制监听，遇到的应用场景：当文本框的输入只允许输入数字（输入身份证号码或金额）。
```java
import org.eclipse.swt.events.VerifyEvent;
import org.eclipse.swt.events.VerifyListener;
public class TextVerifyListener implements VerifyListener{
	private int type;
	public TextVerifyListener(int type){
		this.type=type;
	}
	public void verifyText(VerifyEvent e) {
	    // TODO Auto-generated method stub
	    if(type==1){//只能输入数字
	        boolean b = "0123456789".indexOf(e.text) >= 0 ;
                e.doit = b;  //doit属性如果为true,则字符允许输入,反之不允许
                return;
	    }
	}
}
```
5. 文本框回车键事件监听，应用场景：文本框输入结束后，敲回车键来完成某个事件。其实用到也是上面说到的按键监听事件。
```java
countText1.addKeyListener(new KeyAdapter() {
    @Override
    public void keyPressed(KeyEvent e) {
        if (e.getKeyChar() == KeyEvent.VK_ENTER) {//假如是enter键的话
        bu.login();//要进行相应的事件处理
        }
    }
});
```

### 12. Ant 执行 Java 代码
```java
public class NotifyAdministrator
{
   public static void main(String[] args)
   {
      String email = args[0];
      notifyAdministratorviaEmail(email);
      System.out.println("Administrator "+email+" has been notified");
   }
   public static void notifyAdministratorviaEmail(String email
   { 
       //......
   }
}
```
build.xml
```xml
<?xml version="1.0"?>
<project name="sample" basedir="." default="notify">
   <target name="notify">
      <java fork="true" failonerror="yes" classname="NotifyAdministrator">
         <arg line="admin@test.com"/>
				 <classpath>
				  	<pathelement location="dist/test.jar"/>
					  <pathelement path="${java.class.path}"/>
				 </classpath>
      </java>
   </target>
</project>
```
实际使用中，需要先编译java文件，然后将其加入classpath，才能执行到。   
`<arg value="-l -a"/>` is a single command-line argument containing a space character, not separate options -l and -a.   
`<arg line="-l -a"/>` This is a command line with two separate options, -l and -a.  



----

[Eclipse插件入门-----刷新资源](https://blog.csdn.net/zyf814/article/details/8448209)   
[Java Task in Ant](http://ant.apache.org/manual/Tasks/java.html)   
[Ant 执行 Java 代码](https://www.w3cschool.cn/ant/m24b1hwf.html)   

