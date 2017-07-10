---
layout: post
title: 类型信息
categories: Java
tags: Java
author: NoPerfectName
excerpt: 运行时类型信息使得你可以在程序运行时发现和使用类型信息。
---

* content
{:toc}



## 运行时识别对象和类的信息的方式
（1）RTTI（Run-Time Type Information）：该方式假设我们在编译时已经知道了所有的类型。    
（2）反射：允许我们在运行时发现和使用类的信息。   

## 使用类的准备工作
（1）加载：类加载器加载字节码并创建一个Class对象;  
（2）链接  
（3）初始化：如果该类具有超类，则对其初始化，执行静态初始化器和静态初始化代码。初始化被延迟到对<font color="red">静态方法（构造器隐式地是静态）</font>或者<font color="red">非常数静态域</font>进行首次引用时才执行。
> Class.forName()会进行初始化类
> ClassName.class 则不会初始化
```Java
class B{
	{
		System.out.println("aa");
	}
	static{
		System.out.println("bb");
	}
}
class A extends B{
	public A(){
		System.out.println("init A");
	}
	
	{
		System.out.println("a");
	}
	static{
		System.out.println("b");
	}
	
}

class Test {
	public static void main(String[] args) {
		//new A();
		 try {
            Class.forName("org.netty.test.A");
        }catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
	}
}
输出
new A():
bb
b
aa
a
init A

Class.forName():
bb
b
```
 
