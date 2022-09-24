---
title: leetcode-1492-KthFactor
date: 2022-04-25 19:40:52
summary: 组合总和
categories: leetcode
tags:
- DFS   

---
## n 的第 k 个因子
[leetcode-1492-KthFactor](https://leetcode-cn.com/problems/the-kth-factor-of-n/)


```java
class Solution {
    public int kthFactor(int n, int k) {
        int s = (int) Math.floor(Math.sqrt(n));
        for (int i = 1; i <= s; i ++){
            if (n % i == 0){
                k --;
            }
            if ( k <= 0){
                return i;
            }
        }
        if (s * s == n){
            s --;
        }
        for (int i = s; i >= 1; i --){
            if (n % i == 0){
                k --;
            }
            if ( k <= 0){
                return n / i;
            }
        }
        return -1;
    }
}
```
