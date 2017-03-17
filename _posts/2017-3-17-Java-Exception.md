---
layout: post
title: Java异常机制
categories: Java
tags: Java
author: NoPerfectName
excerpt: 剖析Java的异常结构，深入理解Java的异常机制 。
---

* content
{:toc}


参考：[原文链接](http://m.blog.csdn.net/article/details?id=6155636)
## 基础语法
#### try...catch...finally 语法
```java
try{
	//可能发生异常的代码
}catch(ExceptionType e){
	//捕捉异常
}finally{
	//无论是否发生异常都会执行的语句块
}
```
> **注意**：finally 语句块一定会执行，如果在 try 或 catch 中有 return 语句，finally 语句块会在方法返回之前执行。除了以下 4 种情况：  
1）finally 语句块发生异常;  
2）在前面的代码中用了 System.exit() 退出程序;  
3）程序所在的线程死亡;  
4）关闭CPU;  

![image]({{site.url}}/assets/5.jpg)
<br/>
#### throw、throws
* throw 
用于在代码块中抛出异常，其异常要么由 catch 处理，要么由 throws 继续抛出。  

* throws
用于将方法中发生的异常抛给方法的调用者。使用规则：  
1）如果是不可查异常，即 Error、RunntimeException 或它们的子类，可以不使用 throws 关键字来抛出异常，编译器仍能够顺利通过，异常将由 JVM 抛出。  
2）如果使可查异常，要么由 try-catch 语句捕获，要么用 throws 声明抛出，否则将导致编译错误。  
3）调用方法必须遵循任何可查异常的处理和声明规则。若覆盖一个方法，则不能声明与覆盖方法不同的异常。声明的任何异常必须是被覆盖方法所声明异常的同类或子类。  

```java
void method1() throws IOException{}  //合法  
 
//编译错误，必须捕获或声明抛出IOException  
void method2(){  
  method1();  
}  
 
//合法，声明抛出IOException  
void method3()throws IOException {  
  method1();  
}  
 
//合法，声明抛出Exception，IOException是Exception的子类  
void method4()throws Exception {  
  method1();  
}  
 
//合法，捕获IOException  
void method5(){  
 try{  
    method1();  
 }catch(IOException e){…}  
}  
 
//编译错误，必须捕获或声明抛出Exception  
void method6(){  
  try{  
    method1();  
  }catch(IOException e){throw new Exception();}  
}  
 
//合法，声明抛出Exception  
void method7()throws Exception{  
 try{  
  method1();  
 }catch(IOException e){throw new Exception();}  
} 
```

<br/><br/>
## 异常结构
#### 异常类层次结构
* 结构图

![image]({{site.url}}/assets/6.jpg)
<br/>
* 分类  
 在 Java 中，所有的异常都有一个共同的祖先 Throwable（可抛出）。Throwable 指定代码中可用异常传播机制通过 Java 应用程序传输的任何问题的共性。  
 <br/>
 * Throwable  
 有两个重要的子类：Exception（异常）和 Error（错误），二者都是 Java 异常处理的重要子类，各自都包含大量子类。  
 <br/>
 * Error  
 是程序无法处理的错误，表示运行应用程序中较严重问题。大多数错误与代码编写者执行的操作无关，而表示代码运行时 JVM（Java 虚拟机）出现的问题。  
 <br/>
 * Exception  
Exception 类有一个重要的子类 RuntimeException。RuntimeException 类及其子类表示“JVM 常用操作”引发的错误。例如，若试图使用空值对象引用、除数为零或数组越界，则分别引发运行时异常（NullPointerException、ArithmeticException）和 ArrayIndexOutOfBoundException。
> 注意：异常和错误的区别：异常能被程序本身可以处理，错误是无法处理。 

    1）运行时异常：都是RuntimeException类及其子类异常，如NullPointerException(空指针异常)、IndexOutOfBoundsException(下标越界异常)等，**这些异常是不检查异常**，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。
  
    2）非运行时异常（编译异常）：是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，**如果不处理，程序就不能编译通过。**如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常。

<br/>


* 通常，Java的异常(包括Exception和Error)分为**可查的异常**（checked exceptions）和**不可查的异常**（unchecked exceptions）。  
1)可查异常（编译器要求必须处置的异常）：正确的程序在运行中，很容易出现的、情理可容的异常状况。可查异常虽然是异常状况，但在一定程度上它的发生是可以预计的，而且一旦发生这种异常状况，就必须采取某种方式进行处理。  
&ensp;&ensp;除了RuntimeException及其子类以外，其他的Exception类及其子类都属于可查异常。这种异常的特点是Java编译器会检查它，也就是说，当程序中可能出现这类异常，要么用try-catch语句捕获它，要么用throws子句声明抛出它，否则编译不会通过。   

  2)不可查异常(编译器不要求强制处置的异常):包括运行时异常（RuntimeException与其子类）和错误（Error）。