---
title: leetcode-329-LongestIncreasingPath
date: 2022-04-26 21:40:52
summary: 矩阵中的最长递增路径
categories: leetcode
tags:
- DFS   

---
## 矩阵中的最长递增路径
[leetcode-329-LongestIncreasingPath](https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/)

> DFS

```java
class Solution {
    public static int longestIncreasingPath(int[][] matrix) {
        if (matrix.length == 0){
            return 0;
        }
        int[][] visited = new int[matrix.length][matrix[0].length];
        int res = 0;
        for (int i = 0; i < matrix.length; i ++){
            for (int j = 0; j < matrix[i].length; j ++){
                res = Math.max(res, increasingPath(matrix, visited, i, j, -1));
            }
        }
        return res;
    }
    
    public static int increasingPath(int[][] matrix,int[][] visited, int row, int col, int pre) {
        if (row < 0 || col < 0 || row >= matrix.length || col >= matrix[row].length){
            return 0;
        }
        if (matrix[row][col] <= pre){
            return 0;
        }
        if (visited[row][col] > 0){
            return visited[row][col];
        }
        visited[row][col] = Math.max(Math.max(increasingPath( matrix,visited, row - 1, col, matrix[row][col]),increasingPath( matrix,visited, row + 1, col, matrix[row][col])), 
                Math.max(increasingPath( matrix,visited, row, col - 1, matrix[row][col]),increasingPath( matrix,visited, row, col + 1, matrix[row][col]))) + 1;
        return visited[row][col];
    }
}
```
