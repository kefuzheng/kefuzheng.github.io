---
title: Preference扩展点
key: 2019-05-20
tags: Plug-in
---

### 1. 扩展点
```xml
<extension
    point = "org.eclipse.ui.preferencePages">
    <page
        id="org.eclipse.ui.examples.readmetool.Page1"
        class="org.eclipse.ui.examples.readmetool.ReadmePreferencePage"
        name="%PreferencePage.name">
    </page>
    <page
        id="org.eclipse.ui.examples.readmetool.Page2"
        class="org.eclipse.ui.examples.readmetool.ReadmePreferencePage2"
        name="Readme Example Child Page"
        category="org.eclipse.ui.examples.readmetool.Page1>
    </page>
</extension>
```
扩展点的类必须实现IWorkbenchPreferencePage

### 2. 过滤多余的Preferences
```java
private static final Set<String> ALLOWED_PREFERENCE_PAGES = new HashSet<String>();
static {
  ALLOWED_PREFERENCE_PAGES
			.add("org.eclipse.ui.preferencePages.Workbench");
	ALLOWED_PREFERENCE_PAGES
			.add("org.eclipse.cdt.ui.preferences.CPluginPreferencePage");
	ALLOWED_PREFERENCE_PAGES
			.add("org.eclipse.debug.ui.DebugPreferencePage");
	ALLOWED_PREFERENCE_PAGES
			.add("org.eclipse.egit.ui.GitPreferencesLocal");
}
public static void hideUnWantedPerferencePages() {
	PreferenceManager manager = PlatformUI.getWorkbench()
			.getPreferenceManager();
	IPreferenceNode[] nodes = manager.getRootSubNodes();
	for (IPreferenceNode node : nodes) {
		if (ALLOWED_PREFERENCE_PAGES.contains(node.getId()))
			continue;
		manager.remove(node);
	}
}
```

### 3. Tips
扩展点中变量名称（%Name）的使用  
需要将其变量定义在plugin.properties中，然后文件需要在MANIFEST.MF中注册  
`Bundle-Localization: plugin`  
若不能显示已有plugin的perference，尝试在扩展点中修改其ID

----

[Preferences in the Eclipse Workbench UI](https://www.eclipse.org/articles/article.php?file=Article-Preferences/article.html) 
[Preference pages](https://help.eclipse.org/kepler/index.jsp?topic=/org.eclipse.platform.doc.isv/guide/preferences_prefs_contribute.htm)
[eclipse插件之preference 开发](https://blog.csdn.net/Aqu415/article/details/54645152)
