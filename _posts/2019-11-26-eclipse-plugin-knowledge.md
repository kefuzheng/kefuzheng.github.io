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

### 9.对editor中添加、删除marker
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

### 10. Ant 执行 Java 代码
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
      <java fork="false" failonerror="yes" classname="NotifyAdministrator">
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
### 11. 动态刷新ToolBar的图片
扩展Eclipse的快捷工具栏的时候，经常会碰到一些类似于开关的问题，比如需要根据某个状态或开关量来显示不同的图片，但是IHandler或AbstractHandler只给出了定义回调的执行函数，并没有给出操作UI相关的方法，其实只需要实现一个接口IElementUpdater即可
```java
public class TestAction extends AbstractHandler implements IElementUpdater{
    public static boolean isOn = false;
    private static ImageDescriptor IMAGE_ON = UIActivator.getImageDescriptor("icons/on.gif");
    private static ImageDescriptor IMAGE_OFF = UIActivator.getImageDescriptor("icons/off.gif");

    @Override
    public Object execute(ExecutionEvent event) throws ExecutionException {
        if(!isOn){
            System.out.println("on");
            isOn = true;
        }else{
            System.out.println("off");
            isOn = false;
        }
        Display.getDefault().asyncExec(new Runnable() {
            @Override
            public void run() {
                    ICommandService commandService = PlatformUI.getWorkbench().getActiveWorkbenchWindow().getService(ICommandService.class);
                if(commandService != null){
                    commandService.refreshElements("com.zj.test.ui.commands.on", null);
                }
            }
        });
        return null;
    }

    @Override
    public void updateElement(UIElement element, Map parameters) {
        if(!isOn){
            element.setIcon(IMAGE_ON);
            element.setTooltip("on");
        }else{
            element.setIcon(IMAGE_OFF);
            element.setTooltip("off");
        }
    }
}
```
### 12. SWT中颜色、字体获取
```java
//三种颜色获取方式  
final Color white = new Color(display, 255, 255, 255);  
final Color sysBlack = Display.getCurrent().getSystemColor(SWT.COLOR_BLACK);   
final Color swtBlue = SWTResourceManager.getColor(SWT.COLOR_LIST_SELECTION);  

Font font = new Font(e.display, "Arial", 14, SWT.BOLD | SWT.ITALIC);

// use gc draw line and image
public class CanvasSample {
	private Image image = null;
	private Shell shell = null;
	private Canvas canvas = null;
	
	public CanvasSample(){
		createContent();
	}
	
	public void createContent(){
		shell = new Shell();
		shell.setLayout(new FillLayout());
		image = new Image(shell.getDisplay(), getClass().getResourceAsStream("1.gif"));
		canvas = new Canvas(shell, SWT.NONE);
		canvas.addPaintListener(new PaintListener(){
			public void paintControl(PaintEvent e){
				//绘制图像
				e.gc.drawImage(image, 10, 10);
				//绘制椭圆
				e.gc.drawOval(20, 20, 200, 100);
				//绘制直线
				e.gc.drawLine(20,10,100,20);
				
				Font font = new Font(e.display, "Arial", 14, SWT.BOLD | SWT.ITALIC);
				//绘制字符串
				e.gc.drawString("This is a String", 30, 20);
				
				//设置display前景色
				e.gc.setForeground(e.display.getSystemColor(SWT.COLOR_GREEN));
				//设置display字体
				e.gc.setFont(font);
				//设置display 背景色
				e.gc.setBackground(e.display.getSystemColor(SWT.COLOR_DARK_GRAY));
				//再绘制字符串
				e.gc.drawString("This is a String", 30, 50);
				font.dispose();
			}
		});
		shell.setSize(200, 150);
	}

	public static void main(String[] args){
		Display display = Display.getDefault();
		CanvasSample cSample = new CanvasSample();
		cSample.getShell().open();
		while(!cSample.getShell().isDisposed()){
			if(!display.readAndDispatch())
				display.sleep();
		}
		cSample.getImage().dispose();
		display.dispose();
	}
}
```

### 13. shell的两种获取方式
`Display.getDefault().getActiveShell()` 获取当前窗口的shell   
`PlatformUI.getWorkbench().getActiveWorkbenchWindow().getShell()` 获取当前应用程序的shell


----

[Eclipse插件入门-----刷新资源](https://blog.csdn.net/zyf814/article/details/8448209)   
[Java Task in Ant](http://ant.apache.org/manual/Tasks/java.html)   
[Ant 执行 Java 代码](https://www.w3cschool.cn/ant/m24b1hwf.html)   
[eclipse 资源监听](https://blog.csdn.net/u011893509/article/details/53557415)   
[How You've Changed! - Eclipse IResourceChangeListener](https://blog.csdn.net/davidhsing/article/details/6939273)   

