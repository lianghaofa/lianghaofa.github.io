---
title: leetcode-994-OrangesRotting
date: 2022-04-20 19:56:52
summary: Rotting Oranges
categories: leetcode
tags:
- 图
- BFS
---
## Quick Start
[leetcode-994-OrangesRotting](https://leetcode-cn.com/problems/rotting-oranges/)
### Why BFS?
网格遍历基本套路：
1.DFS
2.BFS
### Here is the code
``` java
public class OrangesRotting {

    public static void main(String[] args) {
        int[][] grid = new int[1][];
        grid[0] = new int[]{1,2};
//        grid[1] = new int[]{1,1,0};
//        grid[2] = new int[]{0,1,1};

        orangesRotting(grid);
    }

    static class Node{
        int row;
        int col;
        int count;
        Node(int row, int col,int count){
            this.row = row;
            this.col = col;
            this.count = count;
        }
    }

    public static int orangesRotting(int[][] grid) {
        if (grid.length == 0){
            return -1;
        }
        int ans = Integer.MIN_VALUE;
        boolean[][] visited = new boolean[grid.length][grid[0].length];
        Deque<Node> queue = new LinkedList<>();
        for (int i = 0; i < grid.length; i ++){
            for (int j = 0; j < grid[i].length ; j ++){
                if (grid[i][j] == 2){
                    queue.add(new Node(i,j,0));
                    visited[i][j] = true;
                }
            }
        }

        while (!queue.isEmpty()){
            Node cur = queue.poll();
            if (cur.row - 1 >= 0 && !visited[cur.row - 1][cur.col] && grid[cur.row - 1][cur.col] == 1){
                visited[cur.row - 1][cur.col] = true;
                queue.add(new Node(cur.row - 1, cur.col, cur.count + 1));
            }
            if (cur.row + 1 < visited.length && !visited[cur.row + 1][cur.col] && grid[cur.row + 1][cur.col] == 1){
                visited[cur.row + 1][cur.col] = true;
                queue.add(new Node(cur.row + 1, cur.col, cur.count + 1));
            }
            if (cur.col - 1 >= 0 && !visited[cur.row][cur.col - 1] && grid[cur.row][cur.col - 1] == 1){
                visited[cur.row][cur.col - 1] = true;
                queue.add(new Node(cur.row, cur.col - 1, cur.count + 1));
            }
            if (cur.col + 1 < visited[cur.row].length && !visited[cur.row][cur.col + 1] && grid[cur.row][cur.col + 1] == 1){
                visited[cur.row][cur.col + 1] = true;
                queue.add(new Node(cur.row, cur.col + 1, cur.count + 1));
            }
            ans = Math.max(ans , cur.count);
        }
        for (int i = 0; i < grid.length; i ++){
            for (int j = 0; j < grid[i].length ; j ++){
                if (grid[i][j] == 1 && !visited[i][j]){
                    return -1;
                }
            }
        }
        return ans;
    }
    
}
```