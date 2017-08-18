---
layout: post
title: Java ReentrantLock
categories: Java
tags: Java
author: NoPerfectName
excerpt: 对ReentrantLock的用法进行介绍
---

* content
{:toc}


### 与synchronized的优势
（1）性能  
重入锁完全可以替代synchronized关键字。在JDK5.0之前，重入锁的性能远远好于synchronized，但从JDK6.0开始，JDK在synchronized上做了大量的优化，使得两者的性能差距并不大。  
</br>

（2）中断响应  
对于synchronized来说，如果一个线程在等待锁，那么结果只有两种情况，要么它获得这把锁继续执行，要么它保持等待。而使用重入锁，则提供了另一种可能，那就是线程可以被中断。也就是在等待锁的过程中，程序可以根据需要取消对锁的请求。  
> public void lockInterruptibly()  throws InterruptedException
```java
public class IntLock implements Runnable{
    public static ReentrantLock lock1 = new ReentrantLock();
    public static ReentrantLock lock2 = new ReentrantLock();
    private int lockNum;

    public IntLock(int lockNum) {
        this.lockNum = lockNum;
    }

    @Override
    public void run() {
        try {
            if (this.lockNum == 1) {
                lock1.lockInterruptibly();
                try {
                    TimeUnit.MILLISECONDS.sleep(500);
                }catch (InterruptedException e) {
                    System.out.println("lock1 sleep");
                }
                lock2.lockInterruptibly();
                System.out.println("lock1 continue...")
            }else {
                lock2.lockInterruptibly();
                try {
                    TimeUnit.MILLISECONDS.sleep(500);
                }catch (InterruptedException e) {
                    System.out.println("lock2 sleep");
                }
                lock1.lockInterruptibly();
            }
        }catch (InterruptedException e) {
            e.printStackTrace();
        }finally {
            if (lock1.isHeldByCurrentThread()) {
                lock1.unlock();
            }
            if (lock2.isHeldByCurrentThread()) {
                lock2.unlock();
            }
            System.out.println(Thread.currentThread().getId() + "线程退出");
        }

    }

    public static void main(String[] args) throws InterruptedException{
        IntLock r1 = new IntLock(1);
        IntLock r2 = new IntLock(2);

        Thread t1 = new Thread(r1);
        Thread t2 = new Thread(r2);

        t1.start();t2.start();
        TimeUnit.MILLISECONDS.sleep(1000);
	//线程t2会释放对lock2的持有，同时放弃对lock1的申请
        t2.interrupt();
    }
}


输出：
java.lang.InterruptedException
	at java.util.concurrent.locks.AbstractQueuedSynchronizer.doAcquireInterruptibly(AbstractQueuedSynchronizer.java:898)
	at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquireInterruptibly(AbstractQueuedSynchronizer.java:1222)
	at java.util.concurrent.locks.ReentrantLock.lockInterruptibly(ReentrantLock.java:335)
	at exercise.IntLock.run(IntLock.java:39)
	at java.lang.Thread.run(Thread.java:745)
11线程退出
lock1 continue
10线程退出

```
使用lockInterruptibly()方法可以解决中断线程，从而解决死锁问题。  
</br>
锁申请等待限时也可以解决死锁问题。  
> public boolean tryLock()
 public boolean tryLock(long timeout, TimeUnit unit) throws InterruptedException

</br>
### Condition 条件
如果理解了Object.wait()和Object.notify()，那么就能很容易理解Condition对象了，它的作用大致与wait()和notify()相同。但是wait()和notify()方法是和synchronized关键字合作使用的，而Condition是与重入锁相关联的。  
Condition接口提供的基本方法如下：
```java
void await() throws InterruptedException
void awaitUninterruptibly()
long awaitNanos(long nanosTimeout) throws InterruptedException
boolean await(long time, TImeUnit unit) throws InterruptedException
void signal()
void signalAll()
```
简单演示COndition的功能 
```java
public class ReenterLockCondition implements Runnable {
    public static ReentrantLock reentrantLock = new ReentrantLock();
    public static Condition condition = reentrantLock.newCondition();

    @Override
    public void run() {
        try {
            reentrantLock.lock();
            condition.await();
            System.out.println("get lock");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            reentrantLock.unlock();
        }
    }

    public static void main(String[] args) throws InterruptedException{
        Runnable r = new ReenterLockCondition();
        Thread thread = new Thread(r);

        thread.start();
        TimeUnit.MILLISECONDS.sleep(1000);

        reentrantLock.lock();
        condition.signal();
        reentrantLock.unlock();
    }
}
```