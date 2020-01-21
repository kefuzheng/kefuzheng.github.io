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


----

[swt table 使用小结](https://blog.csdn.net/inowcome/article/details/6227037)
[JFace中的表格型树TableTreeViewer](https://www.cnblogs.com/DreamDrive/p/4178219.html)
