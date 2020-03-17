---
title: Process in java
key: 2019-05-20
tags: Plug-in
---

### 1. Process
```java
public static void runSystemCmd(String[] cmd, File workingDir){
	ProcessBuilder processBuilder = new ProcessBuilder(cmd);
	processBuilder.directory(workingDir);
	processBuilder = processBuilder.redirectErrorStream(true);
	final Process process;
	try {
		process = processBuilder.start();
	} catch (IOException e) {
		e.printStackTrace();
	}
	InputStreamReader reader = new InputStreamReader(process
				.getInputStream(), "utf-8");
	BufferedReader bf = new BufferedReader(reader);
	String line = "";
	try {
		while ((line = bf.readLine()) != null) {
			System.out.println(line);
		}
	} catch (IOException e) {
		e.printStackTrace();
	}
	
	//write new command
	final OutputStream outputStream = p.getOutputStream();
	try {
		outputStream.write((cmd + "\n").getBytes(Charset.forName("UTF-8")));
		outputStream.flush();
	} catch (Exception e) {
		e.printStackTrace();
	}
}
```
