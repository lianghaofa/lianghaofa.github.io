---
title: leetcode-72-MinDistance
date: 2022-05-09 09:40:52
summary: 编辑距离
categories: leetcode
tags:
- DP

---
## 编辑距离
[leetcode-72-MinDistance](https://leetcode.cn/problems/edit-distance/)

> 对于 word1每个位置进行尝试， 插入  删除  替换  不变。
> 可以发现 插入 替换 都是为了 word1 i1 与 word2 i2的位置相等。 如果插入，替换不相等的字符，代价只能越来越大。
> 建议从暴力递归来理解  dp[i1][i2]
> tranfer0 与 tranfer1 三维DP，不好处理。需要想办法优化.
> 可以发现除了 word0[index0] == word1[index1] ， step 都是 step + 1， 可以优化成向上层返回值的形式 tranfer2

**最重要的是多尝试，能尝试出来就好处理，否则是真的无从下手**
#### 暴力递归到优化

```java
public class MinDistance {

    public static void main(String[] args) {
         // System.out.println(minDistance("horse", "ros"));

         // System.out.println(minDistance("intention", "execution"));

         //System.out.println(minDistance("aae", "abs"));

    }

    static int ans = Integer.MAX_VALUE;
    public static int minDistance(String word1, String word2) {

        char[] w1 = word1.toCharArray();
        char[] w2 = word2.toCharArray();
        //  插入  删除  替换  不变
        int M = word1.length() + 1;
        int N = word2.length() + 1;
        int[][] dp = new int[M][word2.length() + 1];
        for (int i = 0; i < N; i ++){
            dp[M - 1][i] = N - i - 1;
        }
        for (int i = 0; i < M; i ++){
            dp[i][N - 1] = M - i - 1;
        }

        for (int i = M - 2; i >= 0; i --){
            for (int j = N - 2; j >= 0; j --){
                if (w1[i] == w2[j]){
                    dp[i][j] = dp[i + 1][j + 1];
                }else {
                    dp[i][j] = Math.min(dp[i + 1][j + 1],Math.min(dp[i + 1][j], dp[i][j + 1])) + 1;
                }
            }
        }
        return  dp[0][0];
    }

    private static void tranfer0(char[] word0, char[] word1, int index0,int index1, int step){
        if (index0 >= word0.length){
            ans = Math.min(ans, step + (word1.length - index1));
            return;
        }
        if (index1 >= word1.length){
            ans = Math.min(ans, step + (word0.length - index0));
            return;
        }

        //  插入  删除  替换  不变
        //  插入
        tranfer1(word0, word1, index0,index1 + 1, step + 1);

        // 删除
        tranfer1(word0, word1,index0 + 1, index1,step + 1);

        // 替换
        tranfer1(word0, word1,index0 + 1, index1 + 1,step + 1);

        // 不变
        if (word0[index0] == word1[index1]){
            tranfer1(word0, word1,index0 + 1, index1 + 1,step);
        }

    }


    private static void tranfer1(char[] word0, char[] word1, int index0,int index1, int step){
        if (index0 >= word0.length){
            ans = Math.min(ans, step + (word1.length - index1));
            return;
        }
        if (index1 >= word1.length){
            ans = Math.min(ans, step + (word0.length - index0));
            return;
        }
        //  插入  删除  替换  不变
        // 不变
        if (word0[index0] == word1[index1]){
            tranfer1(word0, word1,index0 + 1, index1 + 1,step);
            return;
        }

        //  插入
        tranfer1(word0, word1, index0,index1 + 1, step + 1);

        // 删除
        tranfer1(word0, word1,index0 + 1, index1,step + 1);

        // 替换
        tranfer1(word0, word1,index0 + 1, index1 + 1,step + 1);

    }

    private static int tranfer2(char[] word0, char[] word1, int index0,int index1){
        if (index0 >= word0.length){
            return (word1.length - index1);
        }
        if (index1 >= word1.length){
            return (word0.length - index0);
        }

        if (word0[index0] == word1[index1]){
            return tranfer2(word0, word1,index0 + 1, index1 + 1);
        }

        //  插入  删除  替换  不变

        //  插入
        int r0 = tranfer2(word0, word1, index0,index1 + 1);

        // 删除
        int r1 = tranfer2(word0, word1,index0 + 1, index1);

        // 替换
        int r2 =  tranfer2(word0, word1,index0 + 1, index1 + 1);

        return Math.min(r0, Math.min(r1, r2)) + 1;
    }
}
```
