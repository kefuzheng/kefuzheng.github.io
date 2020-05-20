---
title: Eclipse插件基础知识梳理
key: 2020-05-18
tags: Plug-in
---

### 1. SWT基础
##### 1.Display&Shell
```java
// 1. 创建、获取当前的UI线程
Display display = Display.getDefault();
// 2. 创建窗口（一个shell实例就代表一个窗口）
Shell shell = new Shell(display);
// 3. Display的事件队列和事件循环
while(!shell.isDisposed()){
	if(!display.readAndDispatch()){
		display.sleep();
	}
}
// 图形操作系统会为每一个GUI程序分配一个事件队列，用户操作鼠标或键盘时，操作系统负责将这些事件放到对应程序的事件队列中
```
##### 2.显示器、边界和客户区域
```java
// 获得主显示器对象，显示器对象主要包含边界和客户区域两个部分
// 边界代表了这个显示器的屏幕大小，客户区域则代表可以用来显示窗口屏幕部分大小
Monitor monitor = Display.getDefault().getPrimaryMonitor();
// 边界
monitor.getBounds();
// 客户区域
monitor.getClientArea();
```
##### 3.SWT中的多线程
```java
// 同步调用
Display.syncExec(Runnable runnable)
// 通知UI线程在下一个事件循环中执行runnable参数的run方法。调用这个方法的线程将被阻塞到runnable执行完成为止。

//异步调用
Display.asyncExec(Runnable runnable)
// 通知UI线程在下一个事件循环中执行runnable参数的run方法。调用这个方法的线程将，不会被阻塞， 而且在runnable执行完成后不会得到通知
```

### 2. 控件（Control）
所有的控件类都从org.eclipse.swt.widget.Control继承而来，而Control的父类为Widget。  
Widget是所有和窗口相关的部件的父类，它为所有的窗口组件提纲了创建对象（申请资源）、释放资源和事件监听的功能。  
Widget有两个比较重要的子类，Control类是所有控件的父类；而Item类则是控件的辅助部分，它代表了复杂控件中的某一“项”。  
##### 1.控件的用户数据（getData/setData）
控件的用户数据是开发者自定义的一些存储在控件中的属性，添加或删除属性对控件的使用没有任何影响。
##### 2.控件的释放
控件也会占用系统资源，这部分资源的分配实在控件的构造函数中完成的。  
控件是存在于shell之中的，所有它的父资源是shell控件，当窗口关闭的时候，所有的控件资源都会被释放。  
手动释放控件资源会导致一系列问题，如果不是万不得已，最后不要手动释放一个控件，而应该交给SWT系统自动释放它们。

### 3. 图形资源
SWT中几种常见的图形资源，都继承自org.eclipse.swt.graphics.Resource.
##### 1.Color
```java
// 自己创建的Color对象，不需要时，必须释放
Color color = new Color(Display.getDefault(),255,0,0);
// 使用系统的color不用手动释放
Color cyanColor = Display.getDefault().getSystemColor(SWT.COLOR_CYAN);
```
##### 2.Image
```java
// 自己创建的对象，同样需要手动释放
Image image = new Image(Display.getDefault(), "image_path");
// 使用系统的图片
Image sysImage = Display.getDefault().getSystemImage(SWT.ICON_ERROR);
```
##### 3.Font
Font管理者显示文本时所使用的字体、字号、格式等各种样式。
```java
// 自己创建，需要手动释放
Font font = new Font(Display.getDefault(), "Tahoma", 10, SWT.BOLD);
// 使用系统的
Font sysFont = Display.getDefault().getSystemFont();
// 应用在label上
label.setFont(font);
```
##### 4. SWT中使用Swing
SWT提供了一个org.eclipse.swt.awt.SWT_AWT类（SWT_AWTSWT_AWT桥）
```java
Frame frame = SWT_AWT.new_Frame(composite);
frame.setLayout(new BorderLayout());
JPanel panel = new JPanel();
panel.setLayout(null);
frame.add(panel, BorderLayout.CENTER);
```

### 4. 基本控件和对话框的使用
##### 1. 控件的使用
使用样式SWT.BORDER显示边框，SWT.NONE无边框。  
Label是不能被用户操作的控件，对Label使用样式SWT.SEPARATORSWT.SEPARATOR可以使控件显示成一条水平或竖直的分割线。   
Text只读，SWT.READONLY样式  
SWT.MULTI多行文本框，SWT.WRAP对于多行文本框，可以时文字自动换行。如果使用了SWT.WRAP样式，H_SCROLL样式将被忽略。  
Combo只读，SWT.READONLY样式，并且只能单选，多选可以使用List，样式设为SWT.MULTI。  
CoolBar可以拖动按钮的位置
##### 2. Menu和MenuItem（窗口菜单）
为一个窗口（shell）添加菜单栏时，需要使用Menu的Bar样式，菜单栏只能处于窗口顶部，无法调整位置，并且不能在一个窗口中显示多个菜单栏。  
```java
// 创建菜单栏中的Menu对象
Menu bar = new Menu(shell, SWT.BAR);
shell.setMenuBar(bar);

// 在菜单栏创建一个菜单项
MenuItem fileMenuItem = new MenuItem(bar, SWT.NONE);
fileMenuItem.setText("File");

// 对File创建下拉菜单
final Menu fileMenu = new Menu(fileMenuItem, SWT.PUSH);
fileMenuItem.setMenu(fileMenuItem);
final MenuItem openMenuItem = new MenuItem(fileMenu, SWT.PUSH);
openMenuItem.setText("Open");
final MenuItem exitMenuItem = new MenuItem(fileMenu, SWT.NONE);
exitMenuItem.setText("Exit");

// 对Open 添加子菜单
final Menu openMenu = new Menu(openMenuItem);
openMenuItem.setMenu(openMenu);

final MenuItem dirMenuItem = new MenuItem(openMenu, SWT.PUSH);
dirMenuItem.setText("Directory");
```
MenuItem的样式，CASCADE,PUSH,CHECK,RADIO,SEPRATOR
##### 3. TabFolder和TabItem（选项卡）
```java
//默认选项卡在上端，可以用SWT.BOTTOM显示在下端
final TabFolder tabFolder = new TabFolder(shell, SWT.NONE);
tabFolder.setBounds(10,10,242,234);

final TabItem tab1 = new TabItem(tabFolder, SWT.NONE);
tab1.setText("Tab 1");
//composite存放选项卡中的内容
final Composite comp1 = new Composite(tabFolder, SWT.NONE);
tab1.setControl(comp1);

final TabItem tab2 = new TabItem(tabFolder, SWT.NONE);
tab1.setText("Tab 2");
final Composite comp2 = new Composite(tabFolder, SWT.NONE);
tab2.setControl(comp2);

// 切换选项卡的时候
tabFolder.addSelectionListener(new SelectionAdapter(){
	public void widgetSelected(final SelectionEvent e){
		int index = tabFolder.getSelectionIndex();
		TabItem item = tabFolder.getItem(index);
	}
})
```
##### 4. 消息框
```java
MessageBox mb = new MessageBox(shell, SWT.ICON_ERROR | SWT.OK);
mb.setText("Error");
mb.setMessage("Error message");
int result = mb.open();
if(SWT.OK == result) {

} else {

}
```
图标样式：ICON_ERROR, ICON_WARNING, ICON_INFORMATION, ICON_WORKING, ICON_QUESTION   
按扭组合：SWT.OK|SWT.CANCEL(确定，取消)；SWT.YES|SWT.NO|SWT.CANCEL(是，否，取消)；SWT.ABORT|SWT.RETRY|SWT.INGNORE(终止，忽略，重试)  

##### 5. 文件与目录对话框
文件对话框：FileDialog, 样式为SWT.SAVESWT.SAVE时，表示另存为。OPEN/MULTI样式时，为打开，OPEN单文件，MULTI多文件。   
```java
FileDialog fileDialog = new FileDialog(shell, SWT.OPEN);
// 文件后缀过滤器   
fileDialog.setFilterExtensions(new String[] {".txt", "*.exe"});
// 默认目录  
fileDialog.setFilterPath("path") 
fileDialog.open();
```
目录对话框：DirectoryDialog

### 5. 容器和布局管理器
##### 1. Composite
创建有边框的Composite：  
`Composite composite = new Composite(shell, SWT.BORDER);`   
对于Radio Button，Composite容器默认提供了排他性，也就是在一个composite里面，只能有一个处于选中状态。若使用了NO_RADIO_GROUP样式，两个Radio Button可以同时被选中。  
RIGHT_TO_LEFT样式：会影响到所有包含在容器内的控件。  
EMBERDDED样式：与其他样式不同，使用这种样式的Composite容器通常不用来摆放SWT控件，而是用来显示非SWT（AWT，Swing等）的GUI内容，起到了桥梁作用。   
##### 2. Group
Group是Composite的子类，BORDER，NO_RADIO_GROUP和RIGHT_TO_LEFT等样式，可以直接应用。   
Group会默认显示边框。  
BORDER样式：会显示下凹的边框
##### 3. Shell
Shell是Composite的子类，但是它只能作为顶层的父容器出现，无法嵌套在其他容器内。   
SHELL_TRIM样式：默认的窗口样式，由CLOSE，TITLE，MIN，MAX和RESIZE这几种组成；   
DIALOG_TRIM样式：对话框窗口样式，有TITLE，CLOSE和BORDER组成；
TOOL样式：浮动工具栏样式，标题栏比较窄，字体也要小几号，TOOL和TITLE；   
NO_TRIM样式：去掉所有的样式；  
ON_TOP样式：使窗口永远处于用户桌面的最上层。  
模态(Modality)样式：MODELLESS，默认模态样式，使子窗口不对任何消息进行阻截；PRIMARY_MODAL，使子窗口阻截所有发送到其父窗口的操作消息，如果不管掉这个窗口，就无法对父窗口操作；APPLICATION_MODALAPPLICATION_MODAL，使窗口阻截所有发到与它共享一个DISPLAY实例的窗口的操作消息；SYSTEM_MODAL，它允许窗口拦截操作系统中所有其他窗口的消息，极少使用这种样式。对于同一个Shell容器，只能选择一种静态模式。   
Shell容器的事件：通过添加ShellListener来监听   
Activate事件：Shell成为活动窗口，或窗口被创建时；  
Close事件：Shell被关闭后；  
Deactivate事件：Shell从活动窗口变成非活动窗口；  
Deiconify事件：Shell从最小化状态恢复时；  
Iconify事件：Shell被最小化时。
##### 4. 容器上下文菜单
```java
// 在创建Menu的时候，可以直接调用Menu(Control control)的构造函数，
// 这个构造函数会自己寻找Control实例最上层的Shell容器，并以POP_UP为默认样式
// Menu menu = new Menu(container)效果与下面的实例化一样
Menu menu = new Menu(shell, SWT.POP_UP);
container.setMenu(menu);
MenuItem menuItem1 = new MenuItem(menu, SWT.NONE);
menuItem1.setText("Item1");
```
##### 5. 容器颜色、背景和鼠标指针设置
setBackground设置背景色   
setBackgroundMode设置背景模式：INHERITE_NONE模式，子控件不继承容器的背景色；INHERITE_DEFAULT模式，同样使用了这种模式的子控件才会继承容器的背景色；INHERITE_FORCE模式，强制子控件继承容器的背景色。  
鼠标指针：   
`Cursor handCursor = Display.getSystemCursor(SWT.CURSOR_CROSS); composite.setCursor(handCursor);`
##### 6. 布局管理
`composite.setLayout(new GridLayout())`   
为composite容器安装一个GridLayout布局管理器，安装了之后，需要容器调用layout方法，才能生效，只需要在最高层容器（Shell）调用一次layout()就行。当布局管理器改变时，也需要重新调用layout()方法。     
控件的偏好尺寸（Preferred Size）：控件按照自己的属性，显示出控件最佳的效果，调用Control.pack()方法可以将控件恢复到它的偏好尺寸。  
坐标系：父容器的左上角为原点，横轴为x，纵轴为y，坐标的单位时像素，用4个数字确定一个矩形，(x,y,weight,height)，坐标可为负值   
###### 1. FillLayout布局
保证容器中所有的子控件都显示出来，但是无法手动指定子控件的大小，因此子控件在创建的时候，不需要设置其位置和尺寸。  
```java
// 设置FillLayout布局
// 默认是横向(SWT.HORIZONTAL)，设成纵向：SWT.VERTICAL
FillLayout layout = new FillLayout();
// 间距
layout.spacing = 8;
// 横向页边空白
layout.marginWidth = 10;
// 纵向页边空白
layout.marginHeight = 5;
shell.setLayout(layout);
// 调用layout方法，通知容器开始布局
shell.layout();
```
###### 2. GridLayout布局
此布局管理器会将容器的空间划分为网格（Grid），Grid的列数由属性numColumns指定。行高和列宽均由这个一行/列中最高/宽的控件决定。    
```java
final GridLayout gridlayout = new GridLayout();
gridlayout.numColumns = 2;
shell.setLayout(gridlayout);

final Text addressText = new Text(shell, SWT.BORDER);
addressText.setLayoutData(new GridData(SWT.FILL, SWT.CENTER, true, false));
shell.layout();
```
GridData为GridLayout的布局信息类，入参：horizontalAlignment(水平方向如何对齐)，verticalAlignment(竖直方向如何对齐)，grabExcessHorizontalSpace(是否占用水平方向剩余空间)，grabExcessVerticalSpace(是否占用竖直方向剩余空间)，horizontalSpan(水平跨列数)，verticalSpan(竖直跨行数)   
设置子控件的尺寸：通过Griddata.widthHint和GridData.heightHint来设置。
GridData.exclude:如果设为true，GridLayout将按照AbsoluteLayout(No layout)的方式来布局
###### 3. StackLayout布局
此布局管理的所有子控件中，只有一个显示能够显示出来，通过topControl来指定哪一个子控件当前处于栈顶，然后再调用Composite的layout()进行重新布局，就可以切换处于可见状态的子控件。  
```java
final StackLayout layout = new StackLayout();
composite.setLayout(layout);

layout.topControl = ~
composite.layout();
```

### 6. 高级控件
##### 1. List、Tree和Table
ContentProvider:解析模型，取得查看器所需要的内容；监听数据模型的变化(PropertyChangeListener)   
List: IStructuredContentProvider(inputchanged(),getElements())；LableProvider()   
Table: IStructuredContentProvider(inputchanged(),getElements()); ITableLabelProvider()   
Tree: ITreeContentProvider(inputchanged(),getElements(),getChildren(),getParent(),hasChildren());ITableLabelProvider()   
查看器方法：   
refresh(): 刷新整个查看器的内容   
refresh(Object element): 刷新指定的元素（包含子元素）   
update(Object element, String[] properties): 更新指定的元素，只会更新元素本身显示的内容，properties用于通知查看器该元素中哪些属性发生了变化   
查看器框架中各对象之间的关系：   
a.外部调用setInput为查看器指定模型;   
b.查看器将新的模型传递给内容提供方；   
c.内容提供方再模型上注册监听器，开始监听模型；
d.内容提供方将模型中的元素集合提取出来提供给查看器；
e.查看器将元素交给标签提供方；   
f.标签提供方将元素中用于显示的文字/图片信息取出返回给查看器；
g.查看器把信息显示在控件里。   
样式：   
FULL_SELECTION:整个item都被选中；   
HIDE_SELECTION:窗口失去焦点时，是否还显示选中状态
SIMPLE和MULTI：控制能否同时多行选择   
##### 2. 文本编辑器
```java
Font tahoma = new Font(display, "Tahoma", 16, SWT.BOLD);

StyledText text = new StyledText(shell, SWT.BORDER);
text.setText("The quick brown fox jumps over the lazy dog.");

StyleRange style1 = new StyleRange();
// 从第0个字符开始，长度为9
style1.start = 0;
style1.length = 9;
style1.font = tahoma;
style1.foreground = display.getSystemColor(SWT.COLOR_RED);
// 上移5像素
style1.rise = 5;
// 加下划线
style1.underline = true;
text.setStyleRange(style1);

// 以行为单位，进行文字的调整
// fromline：起始行，lineCount：需要应用该格式的行数
// 首行缩进50像素
style1.setLineIndent(0,1,50);
// 第二行居中
style1.setLineAlignment(2,1,SWT.CENTER);
// 设置段落背景色
style1.setBackground(1,1, new Color());
// 将段落设置成单词间距相等
style1.setLineJustify(3,1, true);
```
##### 3. 滑动条相关
滚动条组成：后退箭头、后翻页、滑动块、前翻页和前进箭头。   
单机前进/后退箭头时，滑动条滑动一行，点击前/后翻页区域时，滚动条滚动一页   
setIncrement/setPageIncremnet设置行/页的宽度   
setThumb设置滑块的大小   
单独使用滚动条，使用滑动条来（Slider）替代   
`Slider slider = new Slider(shell, SWT.HORIZONTAL)`   
##### 4. 进度条与进度指示器
进度条（ProgressBar）：`ProgressBar progressbar = new ProgressBar(this, SWT.NONE)`   
样式：INDETERMINA不停滚动，SMOOTH进度条显示成连续的一条色块  
进度指示器（ProgressIndicator）：以ProcessBar为基础写的，克服了它的一些缺陷   
##### 5. 浏览器
```java
Browser browser = new Browser(shell, SWT.NONE);
browser.setUrl("http://www.eclipse.org")
```
### 7. SWT/JFace的事件处理
##### 1. SWT中的高级事件
| 组件   | 事件                                 | 描述                                                                     |
| :----- | :----------------------------------- | :----------------------------------------------------------------------- |
| Button | Selection                            | Button被选中                                                             |
| Text   | DefaultSelection<br>Modify<br>Verify | 用户再Text中输入回车键时激发<br>Text内容已经被更改<br>Text内容将要被更改 |
| Combo  | DefaultSelection<br>Modify<br>Verify | 用户再Comobo输入回车<br>Combo内容被更改<br>Combo内容将被改变             |



----

[Eclipse插件入门-----刷新资源](https://blog.csdn.net/zyf814/article/details/8448209)

