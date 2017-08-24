---
layout: post
title: Java volatile
categories: Java
tags: JVM
author: NoPerfectName
excerpt: volatile修饰对象
---

* content
{:toc}


volatile能保证顺序性（happens-before）和可见性，不能保证原子性（但是能保证long、double读写的原子性），所以只能在状态独立于程序内其他内容才能使用。
如果使用volatile修饰类成员对象时，当类中的属性发生变化时，也可以使其他线程可见。
如果使用volatile修饰数组，只能保证volatile数组引用的可见性，数组元素的变化并不能保证被其他线程看到。
```java
public class VolatileObjectTest implements Runnable {
    private /*volatile*/ ObjectA a; // 加上volatile 就可以正常结束While循环了
 
    public VolatileObjectTest(ObjectA a) {
        this.a = a;
    }
 
    public ObjectA getA() {
        return a;
    }
 
    public void setA(ObjectA a) {
        this.a = a;
    }
 
    @Override
    public void run() {
        long i = 0;
        while (a.isFlag()) {
            i++;
        }
        System.out.println("stop My Thread " + i);
    }
 
    public void stop() {
        a.setFlag(false);
    }
 
    public static void main(String[] args) throws InterruptedException {
         // 如果启动的时候加上-server 参数则会 输出 Java HotSpot(TM) Server VM
        System.out.println(System.getProperty("java.vm.name"));
         
        VolatileObjectTest test = new VolatileObjectTest(new ObjectA());
        new Thread(test).start();
 
        Thread.sleep(1000);
        test.stop();
        Thread.sleep(1000);
        System.out.println("Main Thread " + test.getA().isFlag());
    }
 
    static class ObjectA {
        private boolean flag = true;
 
        public boolean isFlag() {
            return flag;
        }
 
        public void setFlag(boolean flag) {
            this.flag = flag;
        }
 
    }
}
```
