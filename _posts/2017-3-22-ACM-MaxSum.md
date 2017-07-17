---
layout: post
title: 最大和子串
categories: ACM
tags: ACM
author: NoPerfectName
excerpt: 最大和子串的Java实现
---

* content
{:toc}

```java
/**
 * 问题来源：http://acm.hdu.edu.cn/showproblem.php?pid=1003
 * 采用数学归纳法，假设已知前n项最大和的子串
 */


import java.util.Scanner;

public class MaxSum {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int caseNum = 0;
        if (scanner.hasNext()) {
            caseNum = scanner.nextInt();
        }

        for (int index = 1; index <= caseNum; index++) {
            int integerNum = 0;

            if (scanner.hasNext()) {
                integerNum = scanner.nextInt();
            }

            int result = 0, start = 1, end = 1, temp = 1;
            int seqChild = 0;
            while (temp <= integerNum) {
                if (scanner.hasNextInt()) {
                    int a = scanner.nextInt();
                    if (temp ==1) {
                        result = a;
                        temp++;
                        continue;
                    }
                    seqChild += a;
                    if (seqChild > 0 && result + seqChild >= a) {
                        end = temp;
                        result += seqChild;
                        seqChild = 0;
                    } else if (a > result){
                        start = temp;
                        end = temp;
                        result = a;
                        seqChild = 0;
                    }
                }
                temp++;
            }

            System.out.printf("Case %d:", index);
            System.out.println();
            System.out.printf("%d %d %d",result, start, end);
            System.out.println();

            if (index != caseNum) {
                System.out.println();
            }
        }

    }
}

注：经过多组数据测试均能获得正确答案，但是在oj上一直提示WA，留作日后研究。关键是练习了“数学归纳法”的解题技巧。
```


