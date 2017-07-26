---
layout: post
title: 利用多线程查找最大值
categories: Java
tags: Java
author: NoPerfectName
excerpt: 该实例可以练习下多线程返回值、随机数、线程池的内容。
---

* content
{:toc}



```java
class FindMaxFuture implements Callable<Integer>{
    private int start;
    private int end;
    private int[] array;

    public FindMaxFuture(int start, int end, int[] array) {
        this.start = start;
        this.end = end;
        this.array = array;
    }

    @Override
    public Integer call() {
        int max = Integer.MIN_VALUE;
        for (int i = start; i < end; i++) {
            if (array[i] > max) max = array[i];
        }
        return max;
    }
}

public class FindMax {
    public static void main(String[] args) {
        Random random = new Random();
        final int MAX_SIZE = 20;
	int max = 10;
	int min = 100;

        int[] array = new int[MAX_SIZE];
        for (int i = 0; i < MAX_SIZE; i++) {
            array[i] = random.nextInt(max) % (max-min-1) + min;  //生成 [min,max) 的随机数
        }

        FindMaxFuture f1 = new FindMaxFuture( 0, array.length/2, array);
        FindMaxFuture f2 = new FindMaxFuture(array.length/2, array.length, array);

        ExecutorService executorService = Executors.newFixedThreadPool(2);
        Future<Integer> future1 = executorService.submit(f1);
        Future<Integer> future2 = executorService.submit(f2);
        executorService.shutdown();

        int max = 0;
        try {
            max = Math.max(future1.get(), future2.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
            max = 0;
        }

        System.out.println(max);
    }
}
```
