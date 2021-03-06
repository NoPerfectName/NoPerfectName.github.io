---
layout: post
title: 类型信息
categories: Java
tags: JVM
author: NoPerfectName
excerpt: 运行时类型信息使得你可以在程序运行时发现和使用类型信息。
---

* content
{:toc}



### 运行时识别对象和类的信息的方式
（1）RTTI（Run-Time Type Information）：该方式假设我们在编译时已经知道了所有的类型。    
（2）反射：允许我们在运行时发现和使用类的信息。    
（3）反射作用：获取一个指向某个并不存在于程序空间中的对象引用；远程方法调用（RMI）  
（4）区别：对RTTI来说，编译器在编译时打开和检查.class文件；对反射来说，.class文件在编译时是不可获取的，所以在运行时打开和检查.class文件。

### 动态加载
所有的类都是都是在对其第一次使用时，动态加载到JVM中的。当程序创建第一个对类的静态成员引用时，就会加载这个类。这个证明构造器也是类的静态方法。因此，Java程序在它开始运行之前并非被完全加载，其各个部分是在必需时才加载。这种动态加载的行为，在诸如C++这样的静态加载的语言中是很难或者根本不可能实现。

### 使用类的准备工作
（1）加载：类加载器加载字节码并创建一个Class对象;  
（2）链接  
（3）初始化：如果该类具有超类，则对其初始化，执行静态初始化器和静态初始化代码。初始化被延迟到对<font color="red">静态方法（构造器隐式地是静态）</font>或者<font color="red">非常数静态域</font>进行首次引用时才执行。
> Class.forName()会进行初始化类  
> ClassName.class 则不会初始化  
      
```java
class B{  
	{  
		System.out.println("B");  
	}  
	static{  
		System.out.println("B static");  
	}  
}
class A extends B{  
	public A(){  
		System.out.println("A construtor");
	}  
	
	{
		System.out.println("A");
	}
	static{
		System.out.println("A static");
	}
	
}

public class Test {
	public static void main(String[] args) {
		//new A();
		 try {
           		 Class.forName("A");
        	}catch (ClassNotFoundException e) {
           		 e.printStackTrace();
        	}
	}
}
输出:
new A(): 
B static
A static
B
A
A construtor

Class.forName():
B static
A static
```
 
 ### Class泛型
```java
class A {

}

class B extends A {

}

public class TestReflect {
	public static void main(String[] args) throws Exception{
		Class<? extends A> c = B.class;
		System.out.println(c.getName());  //输出B
		//由于泛型是编译时检查，编译器认为右边表达式的类型为A，所以必须显式转换，否则会报错
		B b = (B)c.newInstance();		
	}
}
```
