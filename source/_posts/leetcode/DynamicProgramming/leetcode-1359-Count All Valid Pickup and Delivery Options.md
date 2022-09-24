---
title: leetcode-1359-Count All Valid Pickup and Delivery Options
date: 2022-05-28 09:40:52
summary: 有效的快递序列数目
categories: leetcode
tags:
- DP

---
## 有效的快递序列数目
[leetcode-1359-Count All Valid Pickup and Delivery Options](https://leetcode.cn/problems/count-all-valid-pickup-and-delivery-options/)



```java
    public int countOrders(int n) {
        if (n == 1){
            return 1;
        }
        long[] dp =  new long[n + 1];
        int mod = (int) (1e9 + 7);
        dp[1] = 1;
        dp[2] = 6;
        for (int i = 3; i <= n; i ++){
            dp[i] = (i *  (2 * i - 1) * dp[i - 1]) % mod;
        }
        return (int) dp[n];
    }
```
