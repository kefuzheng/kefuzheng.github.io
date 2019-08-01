---
title: Change handler to action
key: 2019-08-01
tags: Plug-in
---

### 1. Change handler to action
```java
public class NewAction extends Action {

	private String cmdId;
	private DiscardChangesActionHandler handler;
	
	// name: action name
	// cmdId: handler id
	// handler: abastract handler
	public NewAction(String name, String cmdId, DiscardChangesActionHandler handler) {
		this.cmdId = cmdId;
		this.handler = handler;
		setText(name);
	}

	@Override
	public void runWithEvent(Event event) {
	    // get handler service
		IHandlerService handlerService = PlatformUI.getWorkbench().getService(IHandlerService.class);
		ICommandService commandService = PlatformUI.getWorkbench().getService(ICommandService.class);
		Command command = commandService.getCommand(cmdId);
		ExecutionEvent executionEvent = handlerService.createExecutionEvent(command, event);
		try {
			handler.execute(executionEvent);
		} catch (ExecutionException e) {
			e.printStackTrace();
		}
		super.runWithEvent(event);
	}
	
}
```