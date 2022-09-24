---
title: leetcode-991-Broken Calculator
date: 2022-05-31 15:10:52
summary:  最小体力消耗路径
categories: leetcode
tags:
- 贪心   

---
##  最小体力消耗路径
[leetcode-1631-Path With Minimum Effort](https://leetcode.cn/problems/path-with-minimum-effort/)

> 贪心思想，每次直走代价最小


### 贪心

```java
    class Node{
        int r;
        int c;
        int cost;
        Node(int r, int c, int cost){
            this.r = r;
            this.c = c;
            this.cost = cost;
        }
    }

    public int minimumEffortPath(int[][] heights) {
        int M =  heights.length;
        if (heights.length == 0){
            return 0;
        }
        int N = heights[0].length;
        boolean[][] dp = new boolean[M][N];
        PriorityQueue<Node> min = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                return o1.cost - o2.cost;
            }
        });
        dp[0][0] = true;
        if (N > 1){
            min.add(new Node(0, 1, Math.abs(heights[0][0] - heights[0][1])));
        }
        if (M > 1){
            min.add(new Node(1, 0, Math.abs(heights[0][0] - heights[1][0])));
        }
        int ans = Integer.MIN_VALUE;
        while (!min.isEmpty()){
            Node node = min.poll();
            int r = node.r;
            int c = node.c;
            int val = heights[r][c];
            int cost = node.cost;
            ans = Math.max(cost, ans);
            dp[r][c] = true;
            if (r == M - 1 && c == N - 1){
                break;
            }
            // 把上下左右放进去
            if (r - 1 >= 0 && !dp[r - 1][c]){
                min.add(new Node(r - 1, c, Math.abs(heights[r - 1][c] - val)));
            }

            if (r + 1 < M && !dp[r + 1][c]){
                min.add(new Node(r + 1, c, Math.abs(heights[r + 1][c] - val)));
            }

            if (c - 1 >= 0 && !dp[r][c - 1]){
                min.add(new Node(r, c - 1, Math.abs(heights[r][c - 1] - val)));
            }

            if (c + 1 < N && !dp[r][c + 1]){
                min.add(new Node(r, c + 1, Math.abs(heights[r][c + 1] - val)));
            }
        }
        return ans == Integer.MIN_VALUE ? 0 : ans;
    }
```
