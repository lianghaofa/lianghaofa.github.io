---
title: leetcode-313-Super Ugly Number
date: 2022-05-30 19:40:52
summary: 超级丑数
categories: leetcode
tags:
- DP

---
## 超级丑数
[leetcode-313-Super Ugly Number](https://leetcode.cn/problems/super-ugly-number/)




```java
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {

        int[] ans =  new int[n];
        ans[0] = 1;
        int[] indexs = new int[primes.length];
        int index = 1;
        while (index < n){
            int min = Integer.MAX_VALUE;
            for (int i = 0; i < primes.length; i ++){
                min = Math.min( primes[i] * ans[indexs[i]], min );
            }
            ans[index ++] = min;
            for (int i = 0; i < primes.length; i ++){
                if (primes[i] * ans[indexs[i]] == min){
                    indexs[i] ++;
                }
                index = Math.max(index, indexs[i]);
            }
        }
        return ans[n - 1];
    }
}
```
