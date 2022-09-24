---
title: leetcode-799-Champagne Tower
date: 2022-06-08 15:40:52
summary: 香槟塔
categories: leetcode
tags:
- DP

---
## 香槟塔
[leetcode-799-Champagne Tower](https://leetcode.cn/problems/champagne-tower/)


#### 香槟塔

```java
class Solution {
    public static double champagneTower(int poured, int query_row, int query_glass) {


        double[] map = new double[10001];
        Arrays.fill(map, -1d);
        return Math.min(dp(query_row, query_glass, (double) poured, map), 1);
    }


    private static double dp(int i, int j, double all, double[] map){
        if (i == 0 && j == 0){
            return  all;
        }
        if (map[i * 100 + j] >= 0){
            return map[i * 100 + j];
        }
        if (j == 0){
            double val = dp(i - 1, j, all,  map) - 1;
            double res = val > 0 ? val / 2 : 0;
            map[i * 100 + j] = res;
            return  res;
        }else if (j == i){
            double val = dp(i - 1, j - 1, all,  map) - 1;
            double res = val > 0 ? val / 2 : 0;
            map[i * 100 + j] = res;
            return  res;
        } else {
            double val0 = dp(i - 1, j - 1, all,  map) - 1;
            double res0 = val0 > 0 ? val0 / 2 : 0;
            double val1 = dp(i - 1, j, all,  map) - 1;
            double res1 = val1 > 0 ? val1 / 2 : 0;
            map[i * 100 + j] = res0 + res1;
            return res0 + res1;
        }
    }
}

```
