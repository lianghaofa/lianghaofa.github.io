---
title: 最小公倍数
date: 2022-05-21 21:38:52
summary: 最小公倍数
categories: Math
tags:

---
## 最小公倍数


```java
    private long lcm(long a, long b){
        return a * b / gcd(a, b);
    }

    private long gcd(long a, long b){
        return b == 0 ? a : gcd(b, a % b);
    }
```


