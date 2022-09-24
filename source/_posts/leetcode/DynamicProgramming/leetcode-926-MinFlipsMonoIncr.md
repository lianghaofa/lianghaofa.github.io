---
title: leetcode-926-MinFlipsMonoIncr
date: 2022-05-03 18:40:52
summary: 将字符串翻转到单调递增
categories: leetcode
tags:
- DP   

---
## 将字符串翻转到单调递增
[leetcode-926-MinFlipsMonoIncr](https://leetcode-cn.com/problems/flip-string-to-monotone-increasing/)


```java
    public int minFlipsMonoIncr(String s) {

        //  比较暴力的解
        //  1. 记录结尾到当前位置有多少个0  zeroArr数组
        //  2. 记录开头到当前位置有多少个1  oneCount
        //  3. 遍历字符串，获取  前缀合法 + 后缀合法的结果 + 当前位置合法。   取最小值
        int l = -1;
        while (l + 1 < s.length() && s.charAt(l + 1) == '0'){
            l ++;
        }
        int start = l + 1;
        int zeroCount = 0;
        int[] zeroArr = new int[s.length()];
        for (int i = s.length() - 1; i >= start; i --){
            if (s.charAt(i) == '0'){
                zeroCount ++;
            }
            zeroArr[i] = zeroCount;
        }
        int ans = Integer.MAX_VALUE;
        int oneCount = 0;
        for (int i = start - 1; i < s.length(); i ++){
            int flipCount = (oneCount) + (i + 1 >= s.length() ? 0 : zeroArr[i + 1]);
            if (i >= 0 && s.charAt(i) == '1'){
                flipCount ++;
                oneCount ++;
            }
            ans = Math.min(ans, flipCount);

        }
        return Math.min(oneCount, ans);
    }

    public int minFlipsMonoIncr(String s) {
        // 最终的合法形式
        // 000...000   000....1

        // 预处理下，把前缀0去掉
        int l = -1;
        while (l + 1 < s.length() && s.charAt(l + 1) == '0'){
            l ++;
        }

        // 翻转成 000...000  的代价
        // allZeroCost = 000...000  的代价,  当且仅当 ，s.charAt(i) == '1' 时, allZeroCost 发生变化
        int allZeroCost = 0;

        // 尝试过记录   000...111  的代价，然后比较 allOneCost 与 allZeroCost 的最小值。但非常麻烦
        //int allOneCost = 0;

        // 翻转成 000...1  的代价 与  000... 000  代价,                 两者最小值
        // 当且仅当 , s.charAt(i) == '0' 时, cost 发生变化


        // 问题相当于 ：  dp[i - 1]  推导 ->  dp[i]
        // dp[i - 1] 为当前位置的最小代价

        int cost = 0;
        for (int i = l + 1 ; i < s.length(); i ++){
            if (s.charAt(i) == '1'){
                allZeroCost ++;
            }else {
                cost = Math.min(allZeroCost, cost + 1);
            }

        }
        return cost;
    }
```
