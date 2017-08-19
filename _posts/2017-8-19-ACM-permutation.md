---
layout: post
title: 字符数组的全排列
categories: ACM
tags: ACM
author: NoPerfectName
excerpt: 给定一个字符数组，求出由这些字符排列组成的所有字符串
---

* content
{:toc}


```java

/**
 * 给定一个字符数组，求出由这些字符排列组成的所有字符串
 */

 public void swap(char[] str, int i, int j) {
        char c = str[i];
        str[i] = str[j];
        str[j] = c;
    }

    public void permutation(char[] str) {
        permutation(str, 0, str.length-1);
    }

    public void permutation(char[] str, int start, int end) {
        if (start >= end) {
            System.out.println(str);
        }else {
            for (int i = start; i <= end ; i++) {
                swap(str, start, i);
                permutation(str, start+1, end);
                swap(str, start, i);
            }
        }
    }

```
