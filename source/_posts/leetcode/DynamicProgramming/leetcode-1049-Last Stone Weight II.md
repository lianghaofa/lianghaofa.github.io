---
title: leetcode-1049-Last Stone Weight II
date: 2022-05-12 09:40:52
summary: 最后一块石头的重量 II
categories: leetcode
tags:
- DP

---
## 最后一块石头的重量 II
[leetcode-1049-Last Stone Weight II](https://leetcode.cn/problems/last-stone-weight-ii/)

> 分成最接近的两堆。


```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int stone : stones) {
            sum += stone;
        }
        int N = sum + 1;
        boolean[] dp = new boolean[N];
        dp[0] = true;
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < stones.length; i ++){
            for (int j = dp.length - 1;  j > 0 ; j --){
                if (j - stones[i] >= 0 && dp[j - stones[i]]){
                    dp[j] = true;
                    ans = Math.min(ans, Math.abs((sum - j) - j ));
                }
            }
        }

        return ans;
    }
}
```
