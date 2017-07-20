---
layout: post
title: Java序列化机制
categories: Java
tags: Java
author: NoPerfectName
excerpt: 介绍对象序列化的概念
---

* content
{:toc}


### 序列化的场景
对象昂序列化的概念加入到语言中主要是为了支持两种主要特性  
（1）Java的远程方法调用（RMI）  
（2）对Java Beans来说，对象序列化也是必需的。使用一个Bean时，一般情况下是在设计阶段对它的状态信息进行配置。这种状态信息必须保存下来，并在程序启动时进行后期恢复，这种具体工作由序列化来完成。  
</br></br>

### 序列化和反序列化方法
**序列化对象**  
（1）创建某些OutputStream对象，然后将其封装在一个ObjectOutputStream对象内  
（2）调用writeObject()即可将对象序列化，并将其发送给OutputStream（对象化序列是基于字节）  
**反序列化对象**  
（1）需要将一个InputStream封装到ObjectInputStream内  
（2）调用readObject()  
（3）获得的是一个引用，它指向一个向上转型的Object，所以必须向下转型才能直接设置它们。  
```java
import java.io.*;
 
public class SerializeDemo
{
   public static void main(String [] args)
   {
      Employee e = new Employee("Reyan Ali"，"Phokka Kuan, Ambehta Peer"，11122333，101);
      try
      {
         FileOutputStream fileOut =
         new FileOutputStream("/tmp/employee.ser");
         ObjectOutputStream out = new ObjectOutputStream(fileOut);
         out.writeObject(e);
         out.close();
         fileOut.close();
         System.out.printf("Serialized data is saved in /tmp/employee.ser");
      }catch(IOException i)
      {
          i.printStackTrace();
      }
   }
}
```
```java
import java.io.*;
 
public class DeserializeDemo
{
   public static void main(String [] args)
   {
      Employee e = null;
      try
      {
         FileInputStream fileIn = new FileInputStream("/tmp/employee.ser");
         ObjectInputStream in = new ObjectInputStream(fileIn);
         e = (Employee) in.readObject();
         in.close();
         fileIn.close();
      }catch(IOException i)
      {
         i.printStackTrace();
         return;
      }catch(ClassNotFoundException c)
      {
         System.out.println("Employee class not found");
         c.printStackTrace();
         return;
      }
      System.out.println("Deserialized Employee...");
      System.out.println("Name: " + e.name);
      System.out.println("Address: " + e.address);
      System.out.println("SSN: " + e.SSN);
      System.out.println("Number: " + e.number);
    }
}
```