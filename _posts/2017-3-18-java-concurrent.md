---
layout: post
title: Java并发基础知识
categories: Java
tags: Java
author: NoPerfectName
excerpt: Java并发是一个比较大的话题，这里做基础介绍。
---

* content
{:toc}

## 并发的多面性
用并发解决的问题大致分为两类：  
（1）速度：提高运行在单处理器和多处理器上的程序的性能。  
（2）设计可管理性：Java提供了一个重要的组织结构上的好处，即使程序的设计得到极大地简化，如游戏中仿真的设计。  
编写多线程最基本的困难在于：  
协调不同线程驱动的任务之间对这些资源的使用，以使得这些资源不会同时被多个任务访问。  
<br/><br/>

## 线程池
Executor允许用户管理异步任务的执行，而无须显示地管理线程的生命周期。Executor是启动任务的优先方法。  

<table border="1">
<tr>
    <th>Executor</th>
    <th>线程池的顶级接口</th>
  </tr>

  <tr>
    <th>ExecutorService</th>
    <th>真正的线程池接口</th>
  </tr>
  
  <tr>
    <th>ThreadPoolExecutor</th>
    <th>ExecutorService的默认实现</th>
  </tr>
  
  <tr>
    <th>ScheduledExecutorService</th>
    <th>能和Timer/TimerTask类似，解决那些需要任务重复执行的问题</th>
  </tr>
  
  <tr>
    <th>ScheduledThreadPoolExecutor</th>
    <th>继承ThreadPoolExecutor的ScheduledExecutorService接口实现，周期性任务调度的类实现</th>
  </tr>
</table>

要配置一个线程池是比较复杂的，尤其是对于线程池的原理不是很清楚的情况下，很有可能配置的线程池不是较优的，因此在Executors类里面提供了一些静态工厂，生成一些常用的线程池。  

1  newSingleThreadExecutor  
创建一个单线程的线程池。这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行。  
       
2 newFixedThreadPool  
创建固定大小的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。  
    
3 newCachedThreadPool（通常情况下为首选）  
创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，
那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。  
    
4 newScheduledThreadPool  
创建一个大小无限的线程池。此线程池支持定时以及周期性执行任务的需求。  
<br/><br/>
## 从任务中产生返回值
Runnable是执行工作的独立任务，但是并不返回任何值。Callable接口是一个具有参数类型的泛型，它可以利用方法call()来获得任务的一个返回值，并且必须由ExecutorService.submit()调用。
```java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.*;

class TaskWithResult implements Callable<String> {
    private int id;

    public TaskWithResult(int id) {
        this.id = id;
    }

    @Override
    public String call() throws Exception {
        return "result of TaskWithResult" + id;
    }
}

public class CallableDemo {
    public static void main(String[] args) {
        ExecutorService exec = Executors.newCachedThreadPool();
        List<Future<String>> futureList = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            futureList.add(exec.submit(new TaskWithResult(i)));
        }

        for (Future<String> f : futureList) {
            try {
                System.out.println(f.get());
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (ExecutionException e) {
                e.printStackTrace();
            } finally {
                exec.shutdown();
            }

        }
    }
}

```
<br/><br/>
## 后台线程
所谓后台线程，是指在程序运行的时候在后台提供的一种通用服务的线程，并且这种线程并不属于程序中不可或缺的一部分。因此，当所有的非后台线程结束时，程序也就终止了，同时会杀死所有后台线程。反过来说，只要有任何非后台线程在运行，程序就不会终止。比如，执行main()就是一个非后台线程。
