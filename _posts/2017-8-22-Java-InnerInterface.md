---
layout: post
title: Java内部接口
categories: Java
tags: JVM
author: NoPerfectName
excerpt: Java内部接口的一个小示例
---

* content
{:toc}

```java
interface Outer {
	void out();
	//实现Outer接口的类并不一定需要实现Inner接口
	interface Inner {
		void inner();
	}
}

class OuterImpl implements Outer{
	@Override
	public void out() {
		System.out.println("out");
	}

	public class InnerNode implements Outer.Inner {
		@Override
		public void inner() {
			out();
			System.out.println("inner");
		}
	}
}

public class Test{

	public static void main(String[] args) {
    //类型不能定义为Outer接口类型，否则下面内部类将创建失败
		OuterImpl outer = new OuterImpl();
    //除嵌套类外，初始化内部类，需要先创建外部类对象
		Outer.Inner inner = outer.new InnerNode();
		inner.inner();
	}
}
```
