---
title: leetcode-991-Broken Calculator
date: 2022-05-27 23:10:52
summary: 坏了的计算器
categories: leetcode
tags:
- 贪心   

---
## 坏了的计算器
[leetcode-991-Broken Calculator](https://leetcode.cn/problems/broken-calculator/)


### 暴力解

```java
    public int brokenCalc(int startValue, int target) {

        if (startValue == target){
            return 0;
        }
        int step = 0;
        while (target != startValue){
            if (target > startValue && target % 2 == 0){
                target /= 2;
            }else {
                target += 1;
            }
            step ++;
        }
        return step;
    }
```

### 优化

```java
    public int brokenCalc(int startValue, int target) {

        if (startValue == target){
            return 0;
        }
        int step = 0;
        while (target > startValue){
            if (target % 2 == 0){
                target /= 2;
            }else {
                target += 1;
            }
            step ++;
        }
        return step + (startValue - target);
    }
```
