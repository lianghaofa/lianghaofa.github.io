---
title: 最大公约数
date: 2022-05-21 15:38:52
summary: 最大公约数
categories: Math
tags:

---
## 最大公约数


```java
    private long gcd(long a, long b){
        return b == 0 ? a : gcd(b, a % b);
    }
```


