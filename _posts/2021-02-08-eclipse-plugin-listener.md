---
title: Eclipse插件各种事件监听
key: 2021-02-08
tags: Plug-in
---

### 1.重绘监听(PaintListener)
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

### 2. SWT的Text的几种监听事件
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
### 3. Editor生命周期事件监听(IPartListener)
监听事件：org.eclipse.ui.IPartListener   
```java
public interface IPartListener {
    public void partActivated(IWorkbenchPart part);
    public void partBroughtToTop(IWorkbenchPart part);
    public void partClosed(IWorkbenchPart part);
    public void partDeactivated(IWorkbenchPart part);
    public void partOpened(IWorkbenchPart part);
}
```
实现该接口，然后在编辑器创建完成后在编辑器代码中（如FormEditor的addPage()方法中）加入：
```java
getSite().getPage().addPartListener(this); // 加入监听
getSite().getPage().removePartListener(this); // 在编辑器的dispose()方法中删除监听
```
比如一种应用：在当前编辑器失效（Deactivated）时需要保存编辑器数据，则只需要在partDeactivated()方法中调用doSave()即可。这样做可以避免用户忘记保存编辑器而丢失在编辑器中修改的数据，在用户离开当前编辑器做其他任何操作时编辑器都会自动保存。
### 4. 资源监听(IResourceChangeListener)
监听事件：IResourceChangeListener
```java
IWorkspace workspace = ResourcesPlugin.getWorkspace();
IResourceChangeListener listener = new IResourceChangeListener() {
	public void resourceChanged(IResourceChangeEvent event) {
		IWorkspaceRunnable operation = new IWorkspaceRunnable() {
			public void run(IProgressMonitor monitor) throws CoreException {
				System.out.println("Something changed!");
				// your  operation
			}
		};
		workspace.run(operation, null);
	}
};
workspace.addResourceChangeListener(listener);

//... some time later one ...
workspace.removeResourceChangeListener(listener);
```
使用这种方法的好处是：
1. 避免锁异常，上述两种方式在操作开始前会释放掉工作区的所有锁，操作完成后恢复锁状态。
2. 此方式是一个原子操作，这个过程中发生的资源改变事件只会广播一次，可以减小系统开销，避免监听器改变资源过程中给其他监听器发事件引起的死循环



----

[Eclipse插件入门-----刷新资源](https://blog.csdn.net/zyf814/article/details/8448209)   
[eclipse 资源监听](https://blog.csdn.net/u011893509/article/details/53557415)   
[How You've Changed! - Eclipse IResourceChangeListener](https://blog.csdn.net/davidhsing/article/details/6939273)   

