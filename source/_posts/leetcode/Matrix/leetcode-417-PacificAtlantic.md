---
title: leetcode-417-PacificAtlantic
date: 2022-04-27 18:10:52
summary: 太平洋大西洋水流问题
categories: leetcode
tags:
- DFS
- BFS

---
## 太平洋大西洋水流问题
[leetcode-417-PacificAtlantic](https://leetcode-cn.com/problems/pacific-atlantic-water-flow/)

> DFS BFS
> 此类问题从成功点出发DFS BFS


```java
class Solution {
    public static List<List<Integer>> pacificAtlantic(int[][] heights) {
        List<List<Integer>> lists = new ArrayList<>();
        int r = heights.length;
        int c = heights[0].length;
        boolean[][] dp0 = new boolean[r][c];
        boolean[][] dp1 = new boolean[r][c];

        for (int i = 0; i < heights.length; i ++){
            dp0[i][0] = true;
            dfs0(heights ,i, 0,dp0);
        }

        for (int j = 0; j < heights[0].length; j ++){
            dp0[0][j] = true;
            dfs0(heights ,0, j,dp0);
        }

        for (int i = heights.length - 1; i >= 0; i --){
            dp1[i][heights[0].length - 1] = true;
            dfs0(heights ,i, heights[0].length - 1,dp1);
        }

        for (int j = 0; j < heights[0].length ; j ++){
            dp1[heights.length - 1][j] = true;
            dfs0(heights ,heights.length - 1, j,dp1);
        }
        for (int i = 0; i < r; i++){
            for (int j = 0; j < c; j ++){
                if (dp0[i][j] && dp1[i][j]){
                    List<Integer> list = new ArrayList<>();
                    list.add(i);
                    list.add(j);
                    lists.add(list);
                }
            }
        }
        return lists;
    }

    private static void dfs0(int[][] heights ,int i, int j,boolean[][] can){
        int h = heights[i][j];
        //  上
        if (i - 1 >= 0 && heights[i - 1][j] >= h && !can[i - 1][j]){
            can[i - 1][j] = true;
            dfs0(heights,i - 1,  j,can);
        }
        //  下
        if (i + 1 < heights.length && heights[i + 1][j] >= h && !can[i + 1][j]){
            can[i + 1][j] = true;
            dfs0(heights,i + 1,  j,can);
        }
        //  左
        if (j - 1 >= 0 && heights[i][j - 1] >= h && !can[i][j - 1]){
            can[i][j - 1] = true;
            dfs0(heights , i,  j - 1,can);
        }
        //  右
        if (j + 1 < heights[i].length && heights[i][j + 1] >= h && !can[i][j + 1]){
            can[i][j + 1] = true;
            dfs0( heights,i,  j + 1,can);
        }
    }
}
```
