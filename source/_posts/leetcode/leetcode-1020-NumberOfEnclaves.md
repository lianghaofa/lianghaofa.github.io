---
title: leetcode-1020-NumberOfEnclaves
date: 2022-04-20 19:56:52
summary: 飞地的数量
categories: leetcode
tags:
- DFS
- 递归
---
## LargestPerimeter
[leetcode-1020-NumEnclaves](https://leetcode-cn.com/problems/number-of-enclaves/)
1.遍历四周，与1相连的赋值成0
2.统计剩下1的个数
``` java
public int numEnclaves(int[][] grid) {
        if (grid.length == 0){
            return 0;
        }
        for (int i = 0;i < grid.length; i ++){
            infect(grid, i, 0);
            infect(grid, i, grid[i].length - 1);
        }
        for (int i = 0;i < grid[0].length; i ++){
            infect(grid, 0, i);
            infect(grid, grid.length - 1, i);
        }
        int ans = 0;
        for (int i = 0; i < grid.length; i ++){
            for (int j = 0; j < grid[i].length; j ++){
                if (grid[i][j] == 1){
                    ans ++;
                }
            }
        }
        return ans;
    }

    private void infect(int[][] grid, int row, int col){
        if (row < 0 || col < 0 || row >= grid.length || col >= grid[row].length){
            return;
        }
        if (grid[row][col] == 1){
            grid[row][col] = 0;
            infect(grid, row - 1, col);
            infect(grid, row + 1, col);
            infect(grid, row, col - 1);
            infect(grid, row, col + 1);
        }
    }
```