---
layout: post
title: Java String
categories: Java
tags: Java
author: NoPerfectName
excerpt: 关于String的一些例子
---

* content
{:toc}

```java
	String s1 = "programming"; 
	String s2 = new String("programming"); 
	String s3 = "program"; 
	String s4 = "ming"; 
	String s5 = "program" + "ming"; 
	String s6 = s3 + s4; 
	System.out.println(s1 == s2); 
	System.out.println(s1 == s5); 
	System.out.println(s1 == s6); 
	System.out.println(s1 == s6.intern()); 
	System.out.println(s2 == s2.intern());

结果：
false
true(注意)
false(注意)
true
false
```
