---
layout: post
title: 出现次数最多的字符串
categories: ACM
tags: ACM
author: NoPerfectName
excerpt: 获得出现次数最多的字符串
---

* content
{:toc}


```java

/**
 * 问题来源：http://acm.hdu.edu.cn/showproblem.php?pid=1004
 */

import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class BallonRise {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNextInt()) {
            int ballons = scanner.nextInt();
            if (ballons == 0) break;

            int max = 0;
            String color = null;
            Map<String, Integer> map = new HashMap<>();
            while (ballons-- > 0) {
                String ballonColor = scanner.next();
                int num = map.getOrDefault(ballonColor, 0);
                num ++;
                if (num > max){
                    max = num;
                    color = ballonColor;
                }
                map.put(ballonColor, num);
            }
            System.out.println(color);
        }

    }
}

```
