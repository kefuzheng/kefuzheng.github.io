---
title: TreeViewer实现表格树，排序，右键菜单
key: 2019-11-25
tags: Plug-in
---

### 1. 实现
```java
//强调选取行，在行中的某一栏被选取时，通过将跨字段的整行强调来给用户以整行被选取的视觉指示，可以使用SWT.FULL_SELECTION这个样式即可
//允许选取多行，Table默认情况下只被允许选取一行。可以设定SWT.MULTI样式来允许选取多行。在选取时必须通过按住ctrl键来进行多行选取。
TreeViewer treeViewer = new TreeViewer(scrolledComposite, SWT.BORDER| SWT.H_SCROLL | SWT.V_SCROLL | SWT.FULL_SELECTION);
Tree tree = treeViewer.getTree();
tree.setLinesVisible(true);
tree.setHeaderVisible(true);
tree.setLayoutData(new GridData(SWT.FILL, SWT.FILL, true, true, 1, 1));
treeViewer.setContentProvider(new MyContentProvider);
treeViewer.setLabelProvider(new MyLabelProvider());

TreeViewerColumn treeViewerColumn = new TreeViewerColumn(treeViewer, SWT.NONE);
TreeColumn trclmnModulesAndLoops = treeViewerColumn.getColumn();
trclmnModulesAndLoops.setWidth(100);
trclmnModulesAndLoops.setText("Modules and Loops");

TreeViewerColumn treeViewerColumn_1 = new TreeViewerColumn(treeViewer, SWT.NONE);
TreeColumn treeColumn = treeViewerColumn_1.getColumn();
treeColumn.setWidth(100);
treeColumn.setText("Name");

// 放入的值是hierarchy结构的时候，呈现tree， list的时候，列表呈现
treeViewer.setInput(new Object[0]);
packTree(treeViewer.getTree())
//tree展开
//setAutoExpandLevel 与这个方法实现的功能类似，但是auto这个，每次都会强制展开，不会保留已有的结构
treeViewer.expandToLevel(2);

// 放完数据后，重新布局，改变长度
private void packTree(Tree tree) {
	for (TreeColumn c: tree.getColumns()) {
		c.pack();
	}
}

private Class MyContentProvider implements ITreeContentProvider {

	// 只会在开始的时候访问一次，将数据类型转为array
	@Override
	public Object[] getElements(Object inputElement) {
		if (inputElement instanceof ModuleInfo) {
			return new Object[]{inputElement};
		}
		if (inputElement instanceof Object[]) {
			Object[] array = (Object[])inputElement;
			if(array.length == 0){
				return new ModuleInfo[]{getEmptyModuleInfo()};
			}
			return array;
		}
		return new Object[0];
	}
	
	//获取子节点的数据类型
	@Override
	public Object[] getChildren(Object parentElement) {
		
		if (parentElement instanceof LoopInfo) {
			LoopInfo loop = (LoopInfo) parentElement;
			return loop.getChildrenList().toArray();
		} else if (parentElement instanceof LoopGroup) {
			LoopGroup loopGroup = (LoopGroup) parentElement;
			return loopGroup.getLoops();
		}
		
		return new Object[0];
	
	}

	// 获取父节点，最好重写此方法，父子关系明确，可以调用很多treeviewer自己的功能
	@Override
	public Object getParent(Object element) {

		if (element instanceof ModuleInfo) {
			ModuleInfo currentModuleInfo = (ModuleInfo)element;
			return currentModuleInfo.getParent();
		}
		
		return null;
	}

	@Override
	public boolean hasChildren(Object element) {
		Object[] cs = getChildren(element);
		return cs != null && cs.length > 0;
	}
}

// 还可以根据需要 实现ITableColorProvider, ILabelProvider, ITableFontProvider
private Class MyLabelProvider implements ITableLabelProvider {
    @Override
    public String getColumnText(Object element, int columnIndex) {
        ITreeEntry entry = (ITreeEntry) element;
        switch (columnIndex) {
        case 0:
            return entry.getName();
        case 1:
            List list = entry.getChildren();
            if (list == null)
                return "";
            return String.valueOf(list.size());
        case 2:
            if (entry instanceof People) {
                boolean sex = ((People) entry).isSex();
                return sex ? "男" : "女";
            }
            return "";
        default:
           return "";
        }
    }
    @Override
    public Image getColumnImage(Object element, int columnIndex) {
       return null;
    }
    @Override
    public void addListener(ILabelProviderListener listener) {}
    @Override
    public void removeListener(ILabelProviderListener listener) {}
    @Override
    public void dispose() {}
    @Override
    public boolean isLabelProperty(Object element, String property) {
        return false;
    }
 }
```

### 2. 排序
对了column做监听，当点击column的title的时候，进行排序，只对list的input有效
```java
column.addSelectionListener(new SelectionAdapter() {

	@Override
	public void widgetSelected(SelectionEvent e) {
		
		updateSortColumn((TreeColumn)e.widget, colIndex);
		super.widgetSelected(e);
	}
});

// 排序方法
public void updateSortColumn(TreeColumn column, int colIndex) {
	int direction = getSortDirection(column);
	ViewerComparator sorter = buildColumnSorter(colIndex);
	if (SWT.DOWN == direction) {
		sorter = new InverseSorter(sorter);
	}
	
	tree.setSortColumn(column);
	tree.setSortDirection(direction);
	treeViewer.setComparator(sorter);
}

// 排序箭头的方向
private int getSortDirection(TreeColumn column) {
	if (column != tree.getSortColumn()) {
		return SWT.DOWN;
	}
	return (SWT.DOWN == tree.getSortDirection() ? SWT.UP : SWT.DOWN);
}

// 对不同的column做不同的排序
private ViewerComparator buildColumnSorter(int colIndex) {
	switch (colIndex) {
	case 0:
		return new TripCountSorter();
	case 1:
		return new PipelineTypeSorter();
	default:
		return null;
	}
}

// 实现ViewerComparator的compare方法 来进行比较
private class PipelineTypeSorter extends ViewerComparator {
		
	@Override
	public int compare(Viewer viewer, Object e1, Object e2) {
		return getValue(e1).compareTo(getValue(e2));
	}
	
	public String getValue(Object object) {
		if (object instanceof ModuleInfo) {
			ModuleInfo currentModule = (ModuleInfo)object;
			return currentModule.getPipelineType();
		}
		
		return "-";
	}
}

private class TripCountSorter extends ViewerComparator {
	
	@Override
	public int compare(Viewer viewer, Object e1, Object e2) {
		return Integer.compare(getValue(e1), getValue(e2));
	}
	
	public int getValue(Object object) {
		if (object instanceof LoopInfo) {
			LoopInfo loopInfo = (LoopInfo)object;
			return transformStr2Int(normalize(loopInfo.getTripCount()));
		}
		return -1;
	}
}

// 排序取反
private class InverseSorter extends ViewerComparator {
	private ViewerComparator sorter;
	
	public InverseSorter(ViewerComparator sorter) {
		this.sorter = sorter;
	}

	@Override
	public int compare(Viewer viewer, Object e1, Object e2) {
		return - sorter.compare(viewer, e1, e2);
	}
}
```

### 3. 右键菜单
方式1  
```java
// 右键菜单实现展开、关闭所有子节点
Menu menu = new Menu(tree);
MenuItem expand = new MenuItem(menu, SWT.PUSH);
expand.setText("Expand All");
expand.addSelectionListener(new SelectionAdapter() {

	@Override
	public void widgetSelected(SelectionEvent e) {
		TreeSelection selection = (TreeSelection) treeViewer.getSelection();
		treeViewer.expandToLevel(selection.getFirstElement(), TreeViewer.ALL_LEVELS);
		super.widgetSelected(e);
	}
	
});

MenuItem collapse = new MenuItem(menu, SWT.PUSH);
collapse.setText("Collapse All");
collapse.addSelectionListener(new SelectionAdapter() {

	@Override
	public void widgetSelected(SelectionEvent e) {
		TreeSelection selection = (TreeSelection) treeViewer.getSelection();
		treeViewer.collapseToLevel(selection.getFirstElement(), TreeViewer.ALL_LEVELS);
		super.widgetSelected(e);
	}
	
});

tree.setMenu(menu);
```
方式2  
```java
// 使用已有的action 作为menu item

public class ContextMenuGroup extends ActionGroup {
	@Override
	public void fillContextMenu(IMenuManager mgr) {
		IAction openOldSVAction = new Action() {
			@Override
			public void run() {
				AutoPilotReportUtil.openAnalysisPerspective();
				IFile adb = folder.getFile(name + ".adb");
				new CreatAndOpenCDFGDiagramUIJob(adb).schedule();
			};
		};
		openOldSVAction.setText("Open Old Schedule Viewer");		
		mgr.add(openOldSVAction);
	}
}

private void hookContextMenu() {
	MenuManager menuMgr = new MenuManager("#PopupMenu");
	menuMgr.setRemoveAllWhenShown(true);
	menuMgr.addMenuListener(new IMenuListener() {
		public void menuAboutToShow(IMenuManager manager) {
			ContextMenuGroup actionGroup = new ContextMenuGroup();
			IStructuredSelection selection = (IStructuredSelection) treeViewer
					.getSelection();
			actionGroup.setContext(new ActionContext(selection));
			actionGroup.fillContextMenu(manager);
		}
	});
	Menu menu = menuMgr.createContextMenu(treeViewer.getTree());
	treeViewer.getTree().setMenu(menu);
}
```

### 4. 常见问题
##### 1. 动态的改变column的数量
可以通过设置column.setWidth(0)的方式来实现

##### 2. 表格数据的选择状态
可以通过treeViewer.getSelection() 和setSelection来实现

### 5.在表头和表格内部显示不同的右键菜单
```java
tree.addListener(SWT.MenuDetect, new Listener() {
	public void handleEvent(Event event) {
		Tree t = (Tree) event.widget;
		Point pt = t.getDisplay().map(null, t, event.x, event.y);
		Rectangle clientArea = t.getClientArea();
		boolean isHeader = ((pt.y - clientArea.y) <= t.getHeaderHeight());
		if (isHeader) {
			hookHeaderMenu(t);
		}
		t.setMenu(isHeader ? headerMenu : bodyMenu);
	}
});

private void hookHeaderMenu(Tree tree) {
	if (headerMenu != null) {
		return;
	}
	headerMenu = new Menu(tree);
	MenuItem showAllItem = new MenuItem(headerMenu, SWT.PUSH);
	showAllItem.setText("Show All Hidden Columns");
	showAllItem.addSelectionListener(new SelectionAdapter() {
		@Override
		public void widgetSelected(SelectionEvent e) {
			MenuItem item = (MenuItem) e.widget;
			if (!item.getSelection()) {
				for (Object[] obj : cacheMap.values()) {
					obj[1] = COLUMN_WIDTH;
					obj[3] = true;
				}
				for (MenuItem menuItem : headerMenu.getItems()) {
					menuItem.setSelection(true);
				}
			}
			super.widgetSelected(e);
			packTree(tree);
		}
	});
	new MenuItem(headerMenu, SWT.SEPARATOR);
}
```
### 4. 定制treeItem的背景色，高度，字体颜色等
The following Table events have been defined to provide hooks into the drawing process:  
- SWT.MeasureItem: allows a client to specify the dimensions of a cell's content
- SWT.EraseItem: allows a client to custom draw a cell's background and/or selection, and to influence whether the cell's foreground should be drawn
- SWT.PaintItem: allows a client to custom draw or augment a cell's foreground and/or focus rectangle

### 5. 刷新后恢复之前的展开状态
Object[] expandElements = (Object[]) treeViewer.getExpandedElements();// 获取刷新之前展开的元素  
注意，要在刷新之前获取展开的元素，因为刷新之后默认折叠所有节点。这时不能直接调用treeViewer.setExpandedState(Object, boolean)方法，因为输入模型已经发生变化，恢复展开状态是要展开新的节点，而expandElements存储的是旧的节点，所以要先获取新的节点。
```java
List<Catalog> expandParentCatalogs = new ArrayList<Catalog>();
for (Object expandElement : expandElements) {
	if (expandElement instanceof Catalog) {// 获取刷新之前展开的父目录
               expandParentCatalogs.add((Catalog) expandElement);
        }
}

for (Catalog parentCatalog : parentCatalogs) {// 恢复刷新之前展开的父目录
	for (Catalog expandParentCatalog : expandParentCatalogs) {
		if (expandParentCatalog.getCatalogName().trim().equals(parentCatalog.getCatalogName().trim())) {
			treeViewer.setExpandedState(parentCatalog, true);
			break;
		}
	}
}
```
### 6. table的内容中更改字体和颜色
需求：对于是源文件的字符串，改变字体的颜色，并单击时，打开Ceditor   
```java
private ArrayList<SourcePosition> sourcePositions = new ArrayList<>();
// StyledCellLabelProvider
tableViewerColumn.setLabelProvider(new StyledCellLabelProvider() {
		@Override
		public void update(ViewerCell cell) {
			Object element = cell.getElement();
			if (!(element instanceof List<?>)) {
				super.update(cell);
				return;
			}
			cell.setText(((List<String>)element).get(index).trim());
			String message = cell.getText();

			GC gc = new GC(cell.getControl());
			List<StyleRange> styleRanges = new ArrayList<>();
			String regex = "([^/\\\\<>*?|\"\\s\\(]+.(c|cl|cpp|hpp|h)):(\\d+)";
			Pattern pattern = Pattern.compile(regex);
			Matcher m = pattern.matcher(message);
			while (m.find()) {
				String filename = m.group(1);
				IResource r = null;
				r = AutoPilotReportUtil.getFileResource(filename);
				if (r == null) continue;
				int start = m.start();
				int end = m.end();
				int length = end - start;
				StyleRange s = new StyleRange();
				s.foreground = JFaceColors.getHyperlinkText(cell.getItem().getDisplay());
				s.underline = true;
				s.start = start;
				s.length = length;
				styleRanges.add(s);
				
				int x, y, width, height;
				String result = message.substring(0, start);
				height = gc.textExtent("A").y;
				y = gc.textExtent(result).y - height;
				if(result.contains("\n")){
					int ii = result.lastIndexOf("\n");
					result = result.substring(ii+1);
				}
				x = gc.textExtent(result).x;
				
				width = gc.textExtent(message.substring(start,end)).x;
				Rectangle rectangle = new Rectangle(x, y, width, height);
				String sourceName = filename;
				int lineNumber = Integer.parseInt(m.group(3));
				SourcePosition sourcePosition = new SourcePosition(message, sourceName, lineNumber, rectangle);
				sourcePositions.add(sourcePosition);
			}
			gc.dispose();
			if(styleRanges.size() > 0){
				StyleRange[] rs = new StyleRange[styleRanges.size()];
				styleRanges.toArray(rs);
				cell.setStyleRanges(rs);
			}
			super.update(cell);
		}
	});
}
// 鼠标单击
tableViewerColumn.getViewer().getControl().addMouseListener(new MouseAdapter() {
	@Override
	public void mouseDown(MouseEvent e) {
		Point point = new Point(e.x,e.y);
		ViewerCell cell = tableViewerColumn.getViewer().getCell(point);
		if (cell != null && tblclmnName.getText().equals("Location")) {
			Rectangle rect = cell.getBounds();
			for(SourcePosition sourcePosition : sourcePositions){
				if (sourcePosition.getMessage().equals(cell.getText())) {
					if (sourcePosition.containsPoint(new Point(e.x - rect.x, e.y - rect.y))) {
						String fileName = sourcePosition.getSourceName();
						int lineNumber =  sourcePosition.getLineNumber();
						if(lineNumber == 0 || fileName == null || fileName.isEmpty())
							return;
						source4Rpt.openSourceGotoLine(fileName, lineNumber,null);
						break;
					}
				}
			}
		}
	}
});

public class SourcePosition {
	private String message;
	private String sourceName;
	private int lineNumber;
	private Rectangle rectangle;
	
	public SourcePosition(String message, String sourceName, int lineNumber, Rectangle rectangle) {
		this.message = message;
		this.sourceName = sourceName;
		this.lineNumber = lineNumber;
		this.rectangle = rectangle;
	}
	public String getMessage() return message;
	public String getSourceName() return sourceName;
	public int getLineNumber() return lineNumber;
	public boolean containsPoint(Point point){
		if(rectangle.contains(point)){
			return true;
		}else{
			return false;
		}
	}
}
```
### 7. table上显示不同的控件
```java
private Map<Object, Button> resetBtns = new HashMap<Object, Button>();
// ColumnLabelProvider
treeViewerColumn.setLabelProvider(new ColumnLabelProvider() {
	@Override
	public void update(ViewerCell cell) {
		TreeItem item = (TreeItem) cell.getItem();
		Button btn = resetBtns.get(cell.getElement());
		if (btn == null || btn.isDisposed()) {
			btn = new Button((Composite) cell.getControl(), SWT.NONE);
//					btn.setText("Reset");
			btn.setAlignment(SWT.LEFT);
			GridData gd_btnTest = new GridData(SWT.LEFT, SWT.CENTER, false, false, 1, 1);
			gd_btnTest.widthHint = 20;
			btn.setLayoutData(gd_btnTest);
			ISharedImages sharedImages = PlatformUI.getWorkbench().getSharedImages();
			btn.setImage(sharedImages.getImage(ISharedImages.IMG_TOOL_DELETE));
			resetBtns.put(cell.getElement(), btn);
		}

		TreeEditor editor = new TreeEditor(item.getParent());
//			      editor.grabHorizontal  = true;
		editor.horizontalAlignment = SWT.LEFT;
		editor.minimumWidth = 35;
		editor.setEditor(btn, item, cell.getColumnIndex());
		editor.layout();
	}
	
	@Override
	public String getText(Object element) {
		return "";
	}
});

//在ContentProvider里，做inputchanged监听，当改变时，注销掉
public class TableContentProvider implements ITreeContentProvider {
	@Override
	public void inputChanged(Viewer viewer, Object oldInput, Object newInput) {
		if (((TreeViewer)viewer).getTree() != null && ((TreeViewer)viewer).getTree().getChildren() != null) {
			for (Control item : ((TreeViewer)viewer).getTree().getChildren()) {
				if (item != null && !item.isDisposed()) {
					item.getData();
					item.dispose();
				}
			}
		}
		ITreeContentProvider.super.inputChanged(viewer, oldInput, newInput);
	}
}

// 在展开伸缩时，会有控件不刷新的问题，通过重新setInput来解决
treeViewer.collapseToLevel(selection.getFirstElement(), TreeViewer.ALL_LEVELS);
Object[] expandElements = treeViewer.getExpandedElements();
treeViewer.setInput(treeViewer.getInput());
treeViewer.setExpandedElements(expandElements);
packTree(treeViewer.getTree());
```

----

[swt table 使用小结](https://blog.csdn.net/inowcome/article/details/6227037)
[JFace中的表格型树TableTreeViewer](https://www.cnblogs.com/DreamDrive/p/4178219.html)
[Custom Drawing Table and Tree Items](https://www.eclipse.org/articles/article.php?file=Article-CustomDrawingTableAndTreeItems/index.html)  
[Adding a remove button to a column in a table](https://stackoverflow.com/questions/12480402/adding-a-remove-button-to-a-column-in-a-table)
