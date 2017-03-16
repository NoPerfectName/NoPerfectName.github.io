---
layout: post
title: Java内存机制
categories: Java
tags: Java
author: NoPerfectName
---

* content
{:toc}

对Java的内存机制进行探讨





## Java的运行时内存
![Image_text](https://github.com/NoPerfectName/NoPerfectName.github.io/blob/master/images/java%E5%86%85%E5%AD%98/4.jpg)
#### 线程共享

- Java堆

  存放对象实例，即new的对象。

  

- 方法区

  存储已经被虚拟机加载的类信息、常量、静态变量、JIT编译后的代码等数据。

  

- 运行时常量池

  方法区的一部分。用于存放编译期生成的各种字面量和符号引用。

  

- 直接内存

  NIO、Native函数直接分配的堆外内存。DirectBuffer引用也会使用此部分内存。

  

#### 线程私有

- 程序计数器

  当前线程所执行的字节码的行号指示器

  

- Java虚拟机栈

  Java方法执行的内存模型，每个方法被执行时都会创建一个栈帧，存储局部变量表、操作栈、动态链接、方法出口等信息。

  1）每个线程都有自己独立的栈空间

  2）线程栈只存基本类型和对象地址

  3）方法中局部变量在线程空间中

  

- 本地方法栈

  Native方法服务。在HotSpot虚拟机中和Java虚拟机栈合二为一。

  

## GC算法

* “标记-清除”（Mark-Sweep）算法，分为“标记”和“清除”两个阶段：首先标记出所有需要回收的对象，在标记完成后统一回收掉所有被标记的对象。

![Image_text](https://github.com/NoPerfectName/NoPerfectName.github.io/blob/master/images/java%E5%86%85%E5%AD%98/0.jpg?row=true)

   **它的主要缺点是：**

  1）效率问题，标记和清除过程的效率都不高；

   2）空间问题，标记清除之后会产生大量不连续的内存碎片，空间碎片太多可能会导致，当程序在以后的运行过程中需要分配较大对象时无法找到足够的连续内存而不得不提前触发另一次垃圾收集动作；



* “复制”（Copying）算法，它将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用过的内存空间一次清理掉。这样使得每次都是对其中的一块进行内存回收，内存分配时也就不用考虑内存碎片等复杂情况，只要移动堆顶指针，按顺序分配内存即可，实现简单，运行高效。

![Image_text](https://github.com/NoPerfectName/NoPerfectName.github.io/blob/master/images/java%E5%86%85%E5%AD%98/1.jpg)

**它的主要缺点是：**


1）只是这种算法是将内存缩小为原来的一半，有点过于浪费；

2）对象存活率较高时就要执行较多的复制操作，效率将会变低；



* “标记-整理”（Mark-Compact）算法，标记过程仍然与“标记-清除”算法一样，但后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存，这样话连续的内存空间就比较多了。

![Image_text](https://github.com/NoPerfectName/NoPerfectName.github.io/blob/master/images/java%E5%86%85%E5%AD%98/2.jpg)



* 上面几种算法是通过分代回收(generational collection)混合在一起的，一般是把Java堆分为<font color='#dd0000'>Young Generation（新生代）</font>， <font color='#dd0000'> Old Generation（老年代）</font> 和<font color='#dd0000'>Permanent Generation（持久代）</font>，这样就可以根据各个年代的特点采用最适当的回收算法。

![Image_text](https://github.com/NoPerfectName/NoPerfectName.github.io/blob/master/images/java%E5%86%85%E5%AD%98/3.jpg)

1） 在Young Generation中，有一个叫Eden Space的空间，主要是用来存放新生的对象，还有两个Survivor Spaces（from、to），它们的大小总是一样，它们用来存放每次垃圾回收后存活下来的对象。

2） 在Young Generation块中，垃圾回收一般用Copying的算法，速度快。每次GC的时候，存活下来的对象首先由Eden拷贝到某个SurvivorSpace，当Survivor Space空间满了后，剩下的live对象就被直接拷贝到OldGeneration中去。因此，每次GC后，Eden内存块会被清空。

3） 在Old Generation块中主要存放应用程序中生命周期长的内存对象，垃圾回收一般用mark-compact的算法，速度慢些，但减少内存要求。

4）在Permanent Generation中，主要用来放JVM自己的反射对象，比如类对象和方法对象等。

5） 垃圾回收分多级，0级为全部(Full)的垃圾回收，会回收Old段中的垃圾；1级或以上为部分垃圾回收，只会回收Young中的垃圾，内存溢出通常发生于Old段或Perm段垃圾回收后，仍然无内存空间容纳新的Java对象的情况。

**说明：**

from, to: 这两个区域大小相等，相当于copying算法中的两个区域，当新建对象无法放入eden区时，将出发minor collection。JVM采用copying算法，将eden区与from区的可到达对象复制到to区。经过一次垃圾回收，eden区和from区清空，to区中则紧密的存放着存活对象。随后from区成为新的to区， to区成为新的from区。如果进行minor collection的时候，发现to区放不下，则将部分对象放入成熟世代。另一方面，即使to区没有满，JVM依然会移动世代足够久远的对象到成熟世代。如果成熟世代放满对象，无法移入新的对象，那么将触发major collection（Full回收）。



