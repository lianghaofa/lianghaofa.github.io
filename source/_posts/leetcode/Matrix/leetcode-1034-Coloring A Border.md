---
title: leetcode-1034-Coloring A Border
date: 2022-07-01 20:20:52
summary: 边界着色
categories: leetcode
tags:
- DFS   

---
## 边界着色
[leetcode-1034-Coloring A Border](https://leetcode.cn/problems/coloring-a-border/)



```java
class Solution {
    public static int[][] colorBorder(int[][] grid, int row, int col, int color) {
        int[][] ans = new int[grid.length][grid[0].length];
        int[][] visited = new int[grid.length][grid[0].length];
        // 0 未访问过， 1 访问过， 2边界
        infect(grid, visited, row, col, grid[row][col]);
        for (int i = 0; i < grid.length; i ++){
            for (int j = 0; j < grid[0].length; j ++){
                if (visited[i][j] == 2){
                    ans[i][j] = color;
                }else {
                    ans[i][j] = grid[i][j];
                }
            }
        }
        return ans;
    }

    private static void infect(int[][] grid, int[][] visited, int row, int col, int color){
        if (row < 0 || row >= grid.length || col < 0 || col >= grid[0].length || visited[row][col] > 0){
            return;
        }
        if (grid[row][col] != color){
            return;
        }
        visited[row][col] = 1;
        // row - 1, col
        if (row == 0 || row + 1 == grid.length || col == 0 || col + 1 == grid[0].length){
            visited[row][col] = 2;
        }
        if (row - 1 >= 0){
            if (grid[row - 1][col] == color){
                infect(grid, visited,row - 1, col, color);
            }else {
                visited[row][col] = 2;
            }
        }
        // row + 1, col
        if (row + 1 < grid.length){

            if (grid[row + 1][col] == color){
                infect(grid, visited,row + 1, col, color);
            }else {
                visited[row][col] = 2;
            }
        }
        
        if (col - 1 >= 0){
            if (grid[row][col - 1] == color){
                infect(grid, visited, row, col - 1, color);
            }else {
                visited[row][col] = 2;
            }
        }
        // row , col + 1
        if (col + 1 < grid[0].length){
            if (grid[row][col + 1] == color){
                infect(grid, visited,row, col + 1, color);
            }else {
                visited[row][col] = 2;
            }
        }
    }
}
```
