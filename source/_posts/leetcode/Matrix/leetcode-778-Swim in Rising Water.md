---
title: leetcode-778-Swim in Rising Water
date: 2022-05-28 20:20:52
summary: 水位上升的泳池中游泳
categories: leetcode
tags:
- BFS   

---
## 水位上升的泳池中游泳
[leetcode-778-Swim in Rising Water](https://leetcode.cn/problems/swim-in-rising-water/)



```java
class Solution {
    class Node{
        int height;
        int row;
        int col;
        Node(int row, int col, int height){
            this.row = row;
            this.col = col;
            this.height = height;
        }
    }

    public int swimInWater(int[][] grid) {

        int M = grid.length;
        if (M <= 0){
            return 0;
        }
        int N = grid[0].length;
        boolean[][] visited = new boolean[M][N];
        visited[0][0] = true;
        PriorityQueue<Node> heap = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                return o1.height - o2.height;
            }
        });
        heap.add(new Node(0, 0, grid[0][0]));

        int ans = 0;

        while (!heap.isEmpty()){

            Node node = heap.poll();

            int r = node.row;
            int c = node.col;
            int h = node.height;
            ans = Math.max(ans, h);


            if (r == M - 1 && c == N - 1){
                return ans;
            }
            // left
            if (c - 1 >= 0 && !visited[r][c - 1]){
                heap.add(new Node(r, c - 1, grid[r][c - 1]));
                visited[r][c - 1] = true;
            }
            // right
            if (c + 1 < N && !visited[r][c + 1]){
                heap.add(new Node(r, c + 1, grid[r][c + 1]));
                visited[r][c + 1] = true;
            }
            // top
            if (r - 1 >= 0 && !visited[r - 1][c]){
                heap.add(new Node(r - 1, c, grid[r - 1][c]));
                visited[r - 1][c] = true;
            }
            // down
            if (r + 1 < M && !visited[r + 1][c]){
                heap.add(new Node(r + 1, c, grid[r + 1][c]));
                visited[r + 1][c] = true;
            }
        }

        return ans;
    }
}
```
