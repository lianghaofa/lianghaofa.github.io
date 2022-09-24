---
title: leetcode-790-Domino and Tromino Tiling
date: 2022-05-09 09:40:52
summary: 多米诺和托米诺平铺
categories: leetcode
tags:
- DP

---
## 多米诺和托米诺平铺
[leetcode-790-Domino and Tromino Tiling](https://leetcode.cn/problems/domino-and-tromino-tiling/)


```java
class Solution {
    public int numTilings(int n) {
        if (n <= 2){
            return n;
        }
        int mod = 1000000007;
        long[] dp0 = new long[n];
        long[] dp1 = new long[n];
        long[] dp2 = new long[n];

        // 两层铺满
        dp0[0] = 1;
        dp0[1] = 2;

        // 只铺上层
        dp1[0] = 0;
        dp1[1] = 1;

        // 只铺下层
        dp2[0] = 0;
        dp2[1] = 1;
        for (int i = 2; i < dp0.length; i ++){

            // 两层铺满
            dp0[i] = (dp1[i - 1] + dp2[i - 1] + dp0[i - 1] + dp0[i - 2]) % mod;

            // 只铺上层
            dp1[i] = (dp2[i - 1] + dp0[i - 2]) % mod;
            // 只铺下层
            dp2[i] = (dp1[i - 1] + dp0[i - 2]) % mod;

        }

        return (int) (dp0[n - 1] % mod);
    }
}
```
```java
class Solution {
    public int numTilings(int N) {
        int MOD = 1_000_000_007;
        long[] dp = new long[]{1, 0, 0, 0};
        for (int i = 0; i < N; ++i) {
            long[] ndp = new long[4];
            ndp[0b00] = (dp[0b00] + dp[0b11]) % MOD;
            ndp[0b01] = (dp[0b00] + dp[0b10]) % MOD;
            ndp[0b10] = (dp[0b00] + dp[0b01]) % MOD;
            ndp[0b11] = (dp[0b00] + dp[0b01] + dp[0b10]) % MOD;
            dp = ndp;
        }
        return (int) dp[0];
    }
}
```