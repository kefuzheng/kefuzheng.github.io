---
title: Eclipse CDT
key: 2021-01-08
tags: Plug-in
---

### 1. AST
##### 1. IASTTranslationUnit
方法1：   
```java
 /**
* 创建解析单元
* @param source
* @return
* @throws Exception
*/
static IASTTranslationUnit getTranslationUnit(File source) throws Exception{
    FileContent reader = FileContent.create(
            source.getAbsolutePath(), 
            getContentFile(source).toCharArray());

    
    //C++用GPPLanguage解析，C用GCCLanguage解析
    return GPPLanguage.getDefault().getASTTranslationUnit(
            reader, 
            new ScannerInfo(), 
            IncludeFileContentProvider.getSavedFilesProvider(), 
            null, 
            ILanguage.OPTION_IS_SOURCE_UNIT, 
            new DefaultLogService());
            
    
    // return GCCLanguage.getDefault().getASTTranslationUnit(
    //         reader, 
    //         new ScannerInfo(), 
    //         IncludeFileContentProvider.getSavedFilesProvider(), 
    //         null, 
    //         ILanguage.OPTION_IS_SOURCE_UNIT, 
    //         new DefaultLogService());
}

/**
* 获得文件中的内容
* @param file
* @return
* @throws IOException
*/
static String getContentFile(File file) throws IOException {
    StringBuilder content = new StringBuilder();
    String line;

    try (BufferedReader br = new BufferedReader(
            new InputStreamReader(new FileInputStream(file)))) {
        while ((line = br.readLine()) != null)
            content.append(line).append('\n');
    }

    return content.toString();
}
```
方法二：   
```java
// ITranslationUnit for a workspace file
IPath path= new Path("project/folder/file.c");
IFile file= ResourcesPlugin.getWorkspace().getRoot().getFile(path);
// Create translation unit for file
ITranslationUnit tu= (ITranslationUnit) CoreModel.getDefault().create(file);

// ITranslationUnit for file in the editor
IEditorPart editor = PlatformUI.getWorkbench().getActiveWorkbenchWindow().getActivePage().getActiveEditor();
// Access translation unit of the editor.
ITranslationUnit tu= (ITranslationUnit) CDTUITools.getEditorInputCElement(editor.getEditorInput());

// IIndex for one or more projects
ICProject project= CoreModel.getDefault().getCModel().getCProject("project");
IIndex index= CCorePlugin.getIndexManager().getIndex(project);
// all projects
ICProject[] allProjects= CoreModel.getDefault().getCModel().getCProjects();
index= CCorePlugin.getIndexManager().getIndex(allProjects);

// from cditor
private static IASTTranslationUnit getAST(final CEditor cEditor) {
    try {
        return cEditor.getTU().getAST();
    } catch (CoreException e) {
        e.printStackTrace();
    }
    return null;
}
```
##### 2. parse AST
方法一：   
```java
IASTTranslationUnit u = getTranslationUnit(new File("G:\\temp\\minpath.cpp"));
//输出的是文件的所有内容
System.out.println("simpleDeclaration.getRawSignature():"+u.getRawSignature());

/**
    * 获取注释
    */
IASTComment[]  com = u.getComments();
for (IASTComment iastComment : com) {
    System.out.println("IASTComment:"+iastComment);
}
        
/**
    * 得到预处理语句
    * #include <queue>
    * #define maxnum 120   这一类
    */
IASTPreprocessorStatement[] ps = u.getAllPreprocessorStatements();
for (IASTPreprocessorStatement iastPreprocessorStatement : ps) {
    System.out.println(iastPreprocessorStatement.getRawSignature());
}

//得到文件中定义的声明
IASTDeclaration[] decs = u.getDeclarations();
for ( IASTDeclaration child : decs)
{
    //方法声明
    if (child instanceof IASTFunctionDefinition)
    {
        //获得函数说明符,例void
        System.out.println(((IASTFunctionDefinition)child).getDeclSpecifier().getRawSignature());
        //获得函数的函数声明符 ,例Dijkstra(ALGraph g, int v0, int n)
        System.out.println(((IASTFunctionDefinition)child).getDeclarator().getRawSignature());
        //获得函数体的内容 {}之间的内容
        System.out.println(((IASTFunctionDefinition)child).getBody().getRawSignature());
        
        //输出函数的全部内容
        System.out.println(child.getRawSignature());
        
        //与函数的起始位置有关
        IASTFileLocation  FileLocation = ((IASTFunctionDefinition)child).getFileLocation();
        int startLine = FileLocation.getStartingLineNumber();
        int endLine = FileLocation.getEndingLineNumber();
        System.out.println("length:"+ (endLine-startLine));
    }
}    
```
方法二：   
```java
IASTTranslationUnit translationUnit = getTranslationUnit(file);

ASTVisitor visitor = new ASTVisitor() {
    @Override
    public int visit(IASTDeclaration declaration) {
        // When CDT visit a declaration
        System.out.println("Found a declaration: " + declaration.getRawSignature());
        return PROCESS_CONTINUE;
    }
};
// Enable CDT to visit declaration
visitor.shouldVisitDeclarations = true;
// Adapt visitor with source code unit
translationUnit.accept(visitor);
```
### 2. Complete assit for CEditor
##### 1. org.eclipse.cdt.ui.completionProposalComputer
参考的org.eclipse.cdt.ui plugin.xml, type指定了自动补全的位置和类型
```xml
<extension
      point="org.eclipse.cdt.ui.completionProposalComputer"
      id="HLSCompletionProposalComputer"
      name="HLS Proposal">
      <completionProposalComputer 
        activate="true"
        class="com.autoesl.autopilot.ui.platform.common.HLSCompletionProposalComputer">
    <!--categoryId="org.eclipse.cdt.ui.parserProposalCategory"-->
          <partition type="__dftl_partition_content_type"/>
          <partition type="__c_preprocessor"/>
  </completionProposalComputer>
</extension>
```
```java
public class HLSCompletionProposalComputer implements ICompletionProposalComputer {

	@Override
	public List<ICompletionProposal> computeCompletionProposals(ContentAssistInvocationContext context,
			IProgressMonitor monitor) {
    List<ICompletionProposal> proposals = new ArrayList<ICompletionProposal>();
    String prefix = null;
    int offset = context.getInvocationOffset();		
    if (context instanceof CContentAssistInvocationContext) {
      CContentAssistInvocationContext cContext = (CContentAssistInvocationContext) context;
      IASTCompletionNode completionNode = cContext.getCompletionNode();
      if (completionNode == null)
        return Collections.emptyList();
      prefix = completionNode.getPrefix();
      if (prefix == null) {
        try {
          prefix = cContext.computeIdentifierPrefix().toString();
        } catch (BadLocationException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }
      }
    }
    System.out.println("prefix: " + prefix);
    if (prefix == null || !prefix.startsWith("#pragma")) {
      return proposals;
    }
    int start = offset - prefix.length();		
    String proposal = "#pragma HLS INTERFACE";
    CCompletionProposal test = new CCompletionProposal(proposal, start, proposal.length(), null, "INTERFACE display", RelevanceConstants.HELP_TYPE_RELEVANCE, context.getViewer());
    test.setAdditionalProposalInfo("additional info test1");
    proposals.add(test);
    CCompletionProposal test1 = new CCompletionProposal("#pragma HLS BIND_OP", start, proposal.length(), null, "BIND_OP display", RelevanceConstants.HELP_TYPE_RELEVANCE, context.getViewer());
    test1.setAdditionalProposalInfo("additional info test2");
    proposals.add(test1);
    return proposals;
	}
	@Override
	public List<IContextInformation> computeContextInformation(ContentAssistInvocationContext context,
			IProgressMonitor monitor) {
		// TODO Auto-generated method stub
		return null;
	}
	@Override
	public String getErrorMessage() {
		// TODO Auto-generated method stub
		return null;
	}
	@Override
	public void sessionEnded() {
		// TODO Auto-generated method stub
	}
	@Override
	public void sessionStarted() {
		// TODO Auto-generated method stub
	}
}
```
##### 2. org.eclipse.ui.editors.templates
```xml
<extension
	    point="org.eclipse.ui.editors.templates">		
	<include file="lib/template.xml"/>
</extension>
```
template.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<templates>
<template name="pragambind" description="The pragma for bind" context="org.eclipse.cdt.ui.text.templates.c" id="pragma" enabled="true">#pragma bind fifo -type info
</template>
<template name="pragambind1" description="The pragma for bind" context="org.eclipse.cdt.ui.text.templates.c" id="pragma11" enabled="true">#pragma bind fifo -type info
</template>
<!--  
<template name="if" description="%ifStmt" context="org.eclipse.cdt.ui.text.templates.c" id="org.eclipse.cdt.ui.text.templates.c.if" enabled="true">if (${condition}) {
	${line_selection}${cursor}
}</template>
-->
</templates>
```
### 3. Text Hover in CEditor
##### 1. extension point
```xml
<extension
    point="org.eclipse.cdt.ui.textHovers">
    <hover
        label="pragmaHover"
        description="The description of pragma hover"
        class="com.autoesl.autopilot.ui.platform.common.HLSPragmaHover"
        id="com.autoesl.autopilot.ui.platform.HLSPragmaHover">
    </hover>
</extension>
```
##### 2. TestHover extends AbstractCEditorTextHover
```java
public class TestHover extends AbstractCEditorTextHover{
	@Override
	public String getHoverInfo(ITextViewer textViewer, IRegion hoverRegion) {
		String expression = null;

		if (getEditor() == null)
			return null;
		try {
			IDocument document = textViewer.getDocument();
			if (document == null) {
				return null;
			}
			
			// current world offset
			int offset = hoverRegion.getOffset();
			expression = document.get(offset, hoverRegion.getLength());
			expression = expression.trim();
			if (expression.isEmpty())
				return null;
			
			int line = document.getLineOfOffset(offset);
			int length = document.getLineLength(line);
			int lineOffset = document.getLineOffset(line);
			String lineStr = document.get(lineOffset, length - 1);
			if (!lineStr.startsWith("#pragma")) {
				return null;
			}
			if (lineStr.startsWith("#" + expression)) {
				return null;
			}
			List<String> noemptyList = new ArrayList<String>();
			// remove empty string
			for (String item : lineStr.split(" ")) {
				if (item.isEmpty()) {
					continue;
				}
				noemptyList.add(item);
			}
			if (noemptyList.isEmpty()) {
				return null;
			}
			
			if (noemptyList.size() == 2 && "HLS".equals(expression)){
				return null;
			}
			if (noemptyList.size() > 2 && "HLS".equals(noemptyList.get(1))){
				DirectiveModelBySwig directiveModel = PragmaTemplate.getDirectiveModelByName(noemptyList.get(2));
				if (directiveModel == null) {
					return null;
				}
				// hover on pragma
				if (expression.equals(noemptyList.get(2))) {
					return directiveModel.getDesc();
				}
				// hover on option
				for (int i = 3; i < noemptyList.size(); i++) {
					// boolean option
					if (expression.equals(noemptyList.get(i))) {
						for (OptionModelBySwig optionModel: directiveModel.getOptionModels()) {
							if (optionModel.getName().equals(expression)) {
								return optionModel.getDescription();
							}
						}
					}
					// combox or text option
					if (noemptyList.get(i).contains(expression + "=") || noemptyList.get(i).contains("=" + expression)) {
						String optionName = noemptyList.get(i).split("=")[0];
						for (OptionModelBySwig optionModel: directiveModel.getOptionModels()) {
							if (optionModel.getName().equals(optionName)) {
								return optionModel.getDescription();
							}
						}
						
					}
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
}
```
### 4. Add listenr for CEditor
##### 1. MouseListener
```java
private MouseAdapter mouseListener = new MouseAdapter() {
		
    @Override
    public void mouseDown(MouseEvent e) {
        // TODO Auto-generated method stub
        ISourceViewer sourceViewer = editor.getViewer();
        StyledText styledText =  editor.getViewer().getTextWidget();
        
        // click offset
        int caret = 0;
        if (sourceViewer instanceof ITextViewerExtension5) {
            ITextViewerExtension5 extension = (ITextViewerExtension5) sourceViewer;
            caret = extension.widgetOffset2ModelOffset(styledText.getSelection().x);
        } else {
            int offset = sourceViewer.getVisibleRegion().getOffset();
            caret = offset + styledText.getSelection().x;
        }
    }
};
editor.getViewer().getTextWidget().addMouseListener(mouseListener);
```
##### 2. PostSaveListener
```java
editor.addPostSaveListener(new IPostSaveListener() {
    @Override
    public void saved(ITranslationUnit arg0, IProgressMonitor arg1) {
        // code after saved
    }
};);
```
##### 3. DocumentListener
```java
IEditorInput input = editor.getEditorInput();
IDocumentProvider documentProvider = editor.getDocumentProvider();
IDocument document = documentProvider.getDocument(input);
document.addDocumentListener(new IDocumentListener() {
    @Override
    public void documentAboutToBeChanged(DocumentEvent event) {
    }

    @Override
    public void documentChanged(DocumentEvent event) {
    }
});
```
##### 4. IElementChangedListener
```java
CModelManager.getDefault().addElementChangedListener(new IElementChangedListener() {
			
    @Override
    public void elementChanged(ElementChangedEvent event) {
        // TODO Auto-generated method stub
        ICElementDelta delta = event.getDelta();
        event.getSource();
        processDelta(delta);
    }
});

protected void processDelta(ICElementDelta delta) {
    int kind = delta.getKind();
    delta.getResourceDeltas();
    int flags = delta.getFlags();
    ICElement element = delta.getElement();

    System.out.print("Processing " + element);
    // handle open and closing of a solution or project
    if ((flags & ICElementDelta.F_CLOSED) != 0) {
        System.out.println("  Element Closed");
    }
    if ((flags & ICElementDelta.F_OPENED) != 0) {
        System.out.println("  Element Opened");
    }

    if (kind == ICElementDelta.REMOVED) {
        System.out.println("  Element Removed");
//					removedElements.add(element);			
    }

    if (kind == ICElementDelta.ADDED) {
        System.out.println("  Element Added");
//					addedElements.add(element);			
    }

    if (kind == ICElementDelta.CHANGED) {
        System.out.println("  Element Changed");
        element.getElementName();
        element.getHandleIdentifier();

        if (flags == ICElementDelta.F_MODIFIERS) {
            System.out.println("  Modifiers changed");
        }
        if (flags == ICElementDelta.F_CONTENT) {
            System.out.println("  Contents changed");
        }
        if (flags == ICElementDelta.F_CHILDREN) {
            System.out.println("  Children changed");
        }
    }

    ICElementDelta[] affectedChildren = delta.getAffectedChildren();
    for (int i = 0; i < affectedChildren.length; i++) {
        processDelta(affectedChildren[i]);
    }
}
```
##### 5. SelectionChangedListener
从其他view，再选择editor的时候，会触发这个事件，不是在editor中一直点，一直触发
```java
editor.getSite().getSelectionProvider().addSelectionChangedListener(new ISelectionChangedListener(){
    public void selectionChanged(SelectionChangedEvent event){
    };
});
```


----

[CDT Plug-in Developer Guide](https://help.eclipse.org/2020-03/index.jsp)   
[Eclipse中的CDT解析C++/C的简单使用](https://blog.csdn.net/qq_36198826/article/details/97000882)   
[Eclipse AST(抽象语法树)使用指南](https://wenku.baidu.com/view/a0b8e07931b765ce050814ac.html)    
[Unable to extend completionProposalComputer](https://www.eclipse.org/forums/index.php/t/159166/)   
[【Eclipse 插件开发】拓展 Eclipse XML Editor 的代码提示和补全](https://my.oschina.net/baytars/blog/3152834/print)   
[org.eclipse.wst.jsdt.ui.javaCompletionProposalComputer](https://stackoverflow.com/questions/20779899/content-assist-with-javacompletionproposalcomputer-for-eclipse-jsdt-gives-no-pro)   
[How to make eclipse content assist insert instead of overwrite](https://stackoverflow.com/questions/26453564/how-to-make-eclipse-content-assist-insert-instead-of-overwrite)   
[FAQ How do I add Content Assist to my editor?](https://wiki.eclipse.org/FAQ_How_do_I_add_Content_Assist_to_my_editor%3F)  
[FAQ How do I add Content Assist to my language editor?](https://wiki.eclipse.org/FAQ_How_do_I_add_Content_Assist_to_my_language_editor%3F)  
[FAQ How do I add hover support to my text editor?](https://wiki.eclipse.org/FAQ_How_do_I_add_hover_support_to_my_text_editor%3F)  