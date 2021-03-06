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
有时存在某些情况，**对象**能够在程序不运行的情况下仍能存在并保存其信息。这样，在下次运行程序时，该对象将被重建并且拥有的信息与程序上次运行时它拥有的信息相同。当然，可以通过将信息写入文件或数据库，但是在使万物都成为对象的精神中，如果能够将一个对象声明为“持久性”的，并为我们处理掉所有细节，将会显得非常方便。  
Java的对象序列化将那些实现了Serializable接口的**对象**转换成一个字节序列,并能够在以后将这个字节序列完全恢复为原来的对象。  

对象将序列化的概念加入到语言中主要是为了支持两种主要特性  
（1）Java的远程方法调用（RMI）  
（2）对Java Beans来说，对象序列化也是必需的。使用一个Bean时，一般情况下是在设计阶段对它的状态信息进行配置。这种状态信息必须保存下来，并在程序启动时进行后期恢复，这种具体工作由序列化来完成。  


### 序列化和反序列化方法
**序列化对象**  
（1）创建某些OutputStream对象，然后将其封装在一个ObjectOutputStream对象内  
（2）调用writeObject()即可将对象序列化，并将其发送给OutputStream（对象化序列是基于字节）  
（3）对于Serializable对象，对象完全是以存储的二进制位为基础进行构造，而不调用构造器。
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
         ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("/tmp/employee.ser"));
         out.writeObject(e);
         out.close();
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
         ObjectInputStream in = new ObjectInputStream(new FileInputStream("/tmp/employee.ser"));
         e = (Employee) in.readObject();
         in.close();
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

### 序列化UID
序列化会使类的演变受到限制，这种限制的一个例子与流的唯一标识符有关，即序列版本UID（serial version UID）。  
每个可序列化的类都有一个唯一标识号与它相关联。如果你没有在一个名为serialVersionUID的私有静态final的long域中显示地指定该标识号，系统就会自动地根据这个类来调用一个复杂的运算过程来产生一个标识号。如果你没有显示声明，那么通过任何方式改变类的信息都会使该序列标识号发生变化，从而产生兼容性问题，在运行时会导致InvalidClassException异常。
