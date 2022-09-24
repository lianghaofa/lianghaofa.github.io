---
title: leetcode-2222-Number of Ways to Select Buildings
date: 2022-05-31 20:10:52
summary: 选择建筑的方案数
categories: leetcode
tags:
- DP

---
## 选择建筑的方案数
[leetcode-2222-Number of Ways to Select Buildings](https://leetcode.cn/problems/number-of-ways-to-select-buildings/)


#### 选择建筑的方案数

```java
class Solution {
    public long numberOfWays(String s) {

        // 10 的个数
        long[] oneDp = new long[s.length()];
        // 01 的个数
        long[] zeroDp = new long[s.length()];
        long oneCount = s.charAt(s.length() - 1) == '1' ? 1 : 0;
        long zeroCount = s.charAt(s.length() - 1) == '0' ? 1 : 0;
        // dp[i] 存储 c == '1', 存储 01 的个数，  c == '0'，存储 10 的个数
        // oneCount 1 的个数
        for (int i = s.length() - 2; i >= 0 ; i --){
            char c = s.charAt(i);
            if (c == '0'){
                zeroDp[i] = zeroDp[i + 1] + oneCount;
                oneDp[i] = oneDp[i + 1];
                zeroCount ++;
            }else {
                zeroDp[i] = zeroDp[i + 1];
                oneDp[i] = oneDp[i + 1] + zeroCount;
                oneCount ++;
            }
        }
        
        long ans = 0;
        for (int i = 0; i < s.length() - 2; i ++){
            char c = s.charAt(i);
            if (c == '0'){
                ans += oneDp[i];
            }else {
                ans += zeroDp[i];
            }
        }
        return ans;
    }
}
```
