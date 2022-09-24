---
title: 向量的基本操作
date: 2022-05-15 16:38:52
summary: 向量的基本操作
categories: Math
tags:

---
## 向量的基本操作

### 点积 dot production  - 多维

|a . b| = c

```java

```

### 叉积  cross production  - 3维

a . b = c

c 垂直于 a , b

![cross production](/medias/Math/1652604838.png)


```java
static int ccw(int[] a, int[] b, int[] c) {
        /*
         *  二维叉积公式  a × b = |a| |b| sin(θ) n  or  a * b = x1 * y2 - x2 * y1.
         *
         *  原始向量  ab = (b[0] - a[0],b[1] - a[1])   ac = (c[0] - a[0],c[1] - a[1])
         *
         *  ab 与 ac 叉积 = ab * ac = (b[0] - a[0]) * (c[1] - a[1]) - (c[0] - a[0]) * (b[1] - a[1])
         *
         */
        float cross = (b[0] - a[0]) * (c[1] - a[1]) - (c[0] - a[0]) * (b[1] - a[1]);

        // 顺时针
        if (cross < 0) {
            return -1;
        } // clockwise
        // 反时针
        if (cross > 0) {
            return 1;
        }
        // 共线
        return 0;
    }
```



```java

```


