---
layout: post
title: 需要排序的最小子序列
categories: ACM
tags: ACM
author: NoPerfectName
excerpt: 给定一个数组，找到一个序列，使得只要对这个序列进行排序就可以得到一个有序的数组。
---

* content
{:toc}

```java
public class ShortestUnorderedSequence {
    /*下标小于index
     *离array[index]最近
     *数值小于array[index]
     *返回下标
     **/
    public static int lastLess (int[] array, int index) {
        if (array != null) {
            int num = array[index];
            int i = index;
            while (i > 0) {
                if (array[--i] >= num) {
                    continue;
                }
                return i;
            }
        }
        return -1;
    }
    public static void main(String[] args) {
        int[] a = {1, 2, 5, 6, 4, 8, 7, 3，9};
        int max = Integer.MIN_VALUE;
        int i = 0;
        while (i < a.length) {
            if (a[i] > max ) {
                max = a[i++];
                continue;
            }
            break;
        }
       int start = i-1,end = start;
        for (; i < a.length; i++) {
            if (a[start] >= a[i]) {
                int index = lastLess(a, i);
                if (index >= 0) {
                    start = index + 1;
                }
                end = i;
            } else{
                if (a[i] > max){
                    max = a[i];
                } else {
                    end = i;
                }
            }
        }
        if (start == end) {
            System.out.printf("%d %d",0,0);
        }else {
            System.out.printf("%d %d",start,end);
        }

    }
}
输出：
2 7

```
