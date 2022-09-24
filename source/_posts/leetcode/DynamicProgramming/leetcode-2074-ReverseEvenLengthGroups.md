---
title: leetcode-115-NumDistinct
date: 2022-04-26 14:40:52
summary: 不同的子序列
categories: leetcode
tags:
- DP

---
## 不同的子序列
[leetcode-115-NumDistinct](https://leetcode-cn.com/problems/distinct-subsequences/)


```java
public class NumDistinct {

    public int numDistinct(String s, String t) {
        int l1 = s.length(), l2 = t.length(), ans = 0;
        int[][] dp = new int[l2][l1];
        char[] sArr = s.toCharArray();
        char[] tArr = t.toCharArray();
        for (int i = 0; i < l1; i ++){
            if (sArr[i] == tArr[0]){
                dp[0][i] = 1;
            }
        }
        for (int i = 1; i < l2; i ++){
            int sum = 0;
            for (int j = i; j < l1; j ++){
                sum += dp[i - 1][j - 1];
                if (sArr[j] == tArr[i]){
                    dp[i][j] = sum;
                }
            }
        }
        for (int i = l2 - 1; i < l1; i ++){
            ans += dp[l2 - 1][i];
        }
        return ans;
    }
}
```

