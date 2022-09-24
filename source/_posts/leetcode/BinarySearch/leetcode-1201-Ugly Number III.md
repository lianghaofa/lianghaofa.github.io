---
title: leetcode-1201-Ugly Number III
date: 2022-05-21 21:50:52
summary: 丑数 III
categories: leetcode
- 二分

---
## 丑数 III


[leetcode-1201-Ugly Number III](https://leetcode.cn/problems/ugly-number-iii/)


```java
class Solution {
    public int nthUglyNumber(int n, int a, int b, int c) {
        long ans = -1;
        long ab = lcm( a, b);
        long ac = lcm( a, c);
        long bc = lcm( b, c);
        long abc = lcm(ab, c);
        long l = 1;
        long r = Integer.MAX_VALUE;
        while (l <= r){
            long mid = l + (r - l >> 1);
            long count = mid / a + mid / b + mid / c - ((mid / ab) + (mid / ac) + (mid / bc)) +  (mid / abc);
            if (count >= n){
                ans = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return (int) ans;
    }

    private long gcd(long a, long b){
        return b == 0 ? a : gcd(b, a % b);
    }

    private long lcm(long a, long b){
        return a * b / gcd(Math.max(a, b) , Math.min(a, b));
    }
}
```
