---
title: leetcode-583-Delete Operation for Two Strings
date: 2022-05-23 09:40:52
summary: 两个字符串的删除操作
categories: leetcode
tags:
- DP

---
## 两个字符串的删除操作
[leetcode-583-Delete Operation for Two Strings](https://leetcode.cn/problems/delete-operation-for-two-strings/)


```java
public class MinDistance {


    public static void main(String[] args) {
        System.out.println(minDistance("teacher", "tackers"));
    }

    public static int minDistance(String word1, String word2) {


        char[] longSrt = word1.length() > word2.length() ? word1.toCharArray() : word2.toCharArray();
        char[] shortStr = word1.length() > word2.length() ? word2.toCharArray() : word1.toCharArray();
        int M = shortStr.length;
        int N = longSrt.length;
        int[][] dp = new int[M][N];

        dp[0][0] = shortStr[0] == longSrt[0] ? 1 : 0;
        int l = dp[0][0];
        for (int i = 1; i < N; i ++){
            if (longSrt[i] == shortStr[0]){
                dp[0][i] = 1;
            }else {
                dp[0][i] = dp[0][i - 1];
            }
            l = Math.max(l,dp[0][i] );
        }
        for (int i = 1; i < M; i ++){
            dp[i][0] = shortStr[i] == longSrt[0] ? 1 : 0;
            dp[i][0] = Math.max(dp[i - 1][0], dp[i][0]);
            l = Math.max(l,dp[i][0]);
            for (int j = 1; j < N; j ++){
                if (longSrt[j] == shortStr[i]){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else {

                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
                l = Math.max(l,dp[i][j] );
            }
        }

        // System.out.println(M + "," + N + ", " + l);
        return (M - l) + (N - l);
    }

}
```
