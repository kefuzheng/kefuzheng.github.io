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
	ALLOWED_PREFERENCE_PAGES.add("org.eclipse.ui.preferencePages.Workbench");
	ALLOWED_PREFERENCE_PAGES.add("org.eclipse.cdt.ui.preferences.CPluginPreferencePage");
	ALLOWED_PREFERENCE_PAGES.add("org.eclipse.debug.ui.DebugPreferencePage");
	ALLOWED_PREFERENCE_PAGES.add("org.eclipse.egit.ui.GitPreferencesLocal");
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
### 3. 更改perference的默认值
Preferences页面的默认值，通过Key和Value进行了存贮，若要更改其它插件的Preference的默认值，可以通过代码获取Key，更改Value来进行控制。  
例如：  
![preference_page](https://github.com/kefuzheng/kefuzheng.github.io/raw/master/assets/images/perference_extension.PNG)  
这个是Terminal的Preference，在源代码中可以发现，它的"Shell Command"对应的Key是IPreferenceKeys.PREF_LOCAL_TERMINAL_DEFAULT_SHELL_UNIX，而且此插件（org.eclipse.tm.terminal.view.ui）也给了对应的修改方法  
```java
String value = UIPlugin.getScopedPreferences()
    .getString(IPreferenceKeys.PREF_LOCAL_TERMINAL_DEFAULT_SHELL_UNIX);
if(value!=null){
    UIPlugin.getScopedPreferences()
        .putString(IPreferenceKeys.PREF_LOCAL_TERMINAL_DEFAULT_SHELL_UNIX, "init.sh");
}
```		
若自己的插件没有定义相应的Perference，可以把它设置在org.eclipse.ui.ide插件中,ide是eclipse比较基础的插件
```java
IPreferenceStore store = IDEWorkbenchPlugin.getDefault().getPreferenceStore();
String teString = store.getString(IPreferenceKeys.PREF_LOCAL_TERMINAL_DEFAULT_SHELL_UNIX);
store.setValue(IPreferenceKeys.PREF_LOCAL_TERMINAL_DEFAULT_SHELL_UNIX, "init.sh");
IDEWorkbenchPlugin.getDefault().savePluginPreferences();
```
它们会存在workspace相应的插件setting下面
```xml
./.metadata/.plugins/org.eclipse.core.runtime/.settings/
    org.eclipse.ui.ide.prefs:2:terminals.localTerminalDefaultShellUnix=init.sh
./.metadata/.plugins/org.eclipse.core.runtime/.settings/
    org.eclipse.tm.terminal.view.ui.prefs:2:terminals.localTerminalDefaultShellUnix=init.sh
```
### 4. Tips
扩展点中变量名称（%Name）的使用  
需要将其变量定义在plugin.properties中，然后文件需要在MANIFEST.MF中注册  
`Bundle-Localization: plugin`  
若不能显示已有plugin的perference，尝试在扩展点中修改其ID

----

[Preferences in the Eclipse Workbench UI](https://www.eclipse.org/articles/article.php?file=Article-Preferences/article.html)  
[Preference pages](https://help.eclipse.org/kepler/index.jsp?topic=/org.eclipse.platform.doc.isv/guide/preferences_prefs_contribute.htm)  
[eclipse插件之preference 开发](https://blog.csdn.net/Aqu415/article/details/54645152)
