---
title: 代码自动补全
key: 2020-09-09
tags: Plug-in
---

### 1. Complete assit for Ceditor
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

----

[Unable to extend completionProposalComputer](https://www.eclipse.org/forums/index.php/t/159166/)   
[【Eclipse 插件开发】拓展 Eclipse XML Editor 的代码提示和补全](https://my.oschina.net/baytars/blog/3152834/print)   
[org.eclipse.wst.jsdt.ui.javaCompletionProposalComputer](https://stackoverflow.com/questions/20779899/content-assist-with-javacompletionproposalcomputer-for-eclipse-jsdt-gives-no-pro)   
[How to make eclipse content assist insert instead of overwrite](https://stackoverflow.com/questions/26453564/how-to-make-eclipse-content-assist-insert-instead-of-overwrite)   
[FAQ How do I add Content Assist to my editor?](https://wiki.eclipse.org/FAQ_How_do_I_add_Content_Assist_to_my_editor%3F)  
[FAQ How do I add Content Assist to my language editor?](https://wiki.eclipse.org/FAQ_How_do_I_add_Content_Assist_to_my_language_editor%3F)  
[FAQ How do I add hover support to my text editor?](https://wiki.eclipse.org/FAQ_How_do_I_add_hover_support_to_my_text_editor%3F)  

