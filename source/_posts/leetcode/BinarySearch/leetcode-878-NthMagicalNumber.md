---
title: leetcode-878-NthMagicalNumber
date: 2022-05-20 20:50:52
summary: 第 N 个神奇数字
categories: leetcode
- 二分

---
## 第 N 个神奇数字


[leetcode-878-NthMagicalNumber](https://leetcode.cn/problems/nth-magical-number/)


```java
class Solution {
    public int nthMagicalNumber(int n, int a, int b) {
        int MOD = 1_000_000_007;
        long l = 1;
        long r = (long) 1e15;
        long ans = -1;
        long L = lcm(a, b);
        while (l <= r){
            long mid = l + (r - l >> 1);
            long count = mid / a + mid / b -  mid / L;
           if (count >= n){
               ans = mid;
               r = mid - 1;
           }else {
               l = mid + 1;
           }
        }
        return (int) (ans % MOD);
    }

    private long lcm(long a, long b){
        return a * b / gcd(a, b);
    }

    private long gcd(long a, long b){
        return b == 0 ? a : gcd(b, a % b);
    }

}
```
