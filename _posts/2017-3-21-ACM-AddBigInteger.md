---
layout: post
title: 大正整数相加
categories: ACM
tags: ACM
author: NoPerfectName
excerpt: 大正整数相加的Java实现
---

* content
{:toc}

```java
import java.math.BigInteger;
import java.util.Scanner;

/**
 * 大正整数相加
 */

public class AddBigNumber {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int loop = 0;
        if (scanner.hasNext()) {
            loop = scanner.nextInt();
        }
        for (int i = 1; i <= loop; i++) {
            if (scanner.hasNext()) {
//                BigInteger a = scanner.nextBigInteger();
//                BigInteger b = scanner.nextBigInteger();
//                System.out.println("Case " + i + ":");
//                System.out.println(a.toString() + " + " + b.toString() + " = " + a.add(b).toString());

                String a = scanner.next();
                String b = scanner.next();

                StringBuffer result = new StringBuffer();

                int len1 = a.length();
                int len2 = b.length();

                int carry = 0;

                while (len1 > 0 || len2 > 0) {
                    len1--;
                    len2--;

                    int x = 0;
                    int y = 0;
                    if (len1 >= 0) {
                        x = a.charAt(len1) - '0';
                    }
                    if (len2 >= 0) {
                       y = b.charAt(len2) - '0';
                    }

                    int sum = x + y + carry;
                    carry = sum / 10;
                    result.append(sum % 10);

                }

                if (carry != 0) {
                    result.append(carry);
                }

                System.out.println("Case " + i + ":");
                System.out.println(a + " + " + b + " = " + result.reverse().toString());

                if (i != loop) {
                    System.out.println();
                }
            }
        }
    }
}

```