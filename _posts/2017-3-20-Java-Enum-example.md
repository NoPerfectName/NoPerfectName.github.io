---
layout: post
title: Java枚举
categories: Java
tags: Java
author: NoPerfectName
excerpt: 一个枚举的例子，内容涉及了枚举以及流式操作。
---

* content
{:toc}


```java
import java.util.*;

enum Fruit {
	NONE(0),APPLE(1),ORANGE(2);

	private int index;
	private static Map<Integer, Fruit> fruitMap = new HashMap<>();

	static {
		for (Fruit f : Fruit.values()) {
			fruitMap.put(f.getIndex(), f);
		}
	}

	private Fruit(int index) {
		this.index = index;
	}

	private int getIndex() {
		return this.index;
	}

	public void printIndex() {
		System.out.println(index);
	}

	public static Fruit getFruitByIndex1(int index) {
		return fruitMap.getOrDefault(index, Fruit.NONE);
	}

	//another method to get Fruit by Index
	public static Fruit getFruitByIndex2(int index) {
		return Arrays.stream(Fruit.values()).filter(v -> v.getIndex() == index).findAny().orElse(Fruit.NONE);
	}

}
public class Hello{
	
	public static void main(String[] args) throws Exception{
		Fruit.getFruitByIndex1(1).printIndex();
		Fruit.getFruitByIndex1(2).printIndex();
	}
}

```
