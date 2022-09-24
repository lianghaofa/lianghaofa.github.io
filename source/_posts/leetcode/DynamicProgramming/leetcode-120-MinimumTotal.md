---
title: leetcode-120-MinimumTotal
date: 2022-05-05 14:40:52
summary: 三角形最小路径和
categories: leetcode
tags:
- DP

---
## 三角形最小路径和
[leetcode-120-MinimumTotal](https://leetcode-cn.com/problems/triangle/)

#### 自顶向下

```java
    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle.size() == 0){
            return 0;
        }
        for (int i = 1; i < triangle.size(); i ++){
            List<Integer> curList = triangle.get(i);
            List<Integer> preList = triangle.get(i - 1);
            for (int j = 0; j < curList.size(); j++){
                if (j == 0){
                    curList.set(j,curList.get(j) + preList.get(j));
                }else if (j == curList.size() - 1){
                    curList.set(j,curList.get(j) + preList.get(j - 1));
                }else {
                    curList.set(j,curList.get(j) + Math.min(preList.get(j - 1),preList.get(j)));
                }
            }
        }
        int min = Integer.MAX_VALUE;
        List<Integer> curList = triangle.get(triangle.size() - 1);
        for (Integer integer : curList) {
            min = Math.min(integer, min);
        }
        return min;
    }
```

#### 自低向上

```java
    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle.size() == 0 || triangle.get(0).size() == 0){
            return -1;
        }
        int[] dp = new int[triangle.size() + 1];
        for (int i = triangle.size() - 1; i >= 0; i --){
            for (int j = 0;j <= i ; j ++){
                dp[j] = Math.min(dp[j],dp[j + 1]) + triangle.get(i).get(j);
            }
        }
        return dp[0];
    }
```