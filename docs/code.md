---
title: 算法习题
key: 2020-05-18
tags: 算法
---

### 1. 无重复字符的最长子串
给定一个字符串，请你找出其中不含有重复字符的最长子串的长度
```java
public static int getMaxSubStr(String str){
  String maxStr = "";
  String tmpStr = "";
  for(int i=0; i < str.length(); i++>){
    if(tmpStr.contains(str.charAt(i))){
      if (maxStr.length() < tmpStr.length()) {
        maxStr = tmpStr;
        tmpStr = tmpStr.splite(str.charAt(i))[1] + str.charAt(i);
      }
    } else {
      tmpStr = tmpStr + str.charAt(i);
    }
  }
  if(tmpStr.length() > maxStr.length()) {
    return tmpStr.length();
  }
  return maxStr.length();
}
```