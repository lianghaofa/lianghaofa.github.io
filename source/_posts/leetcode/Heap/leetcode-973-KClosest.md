---
title: leetcode-973-KClose
date: 2022-04-29 19:40:52
summary: 最接近原点的 K 个点
categories: leetcode
tags:
- 堆   

---
## 最接近原点的 K 个点
[leetcode-973-CombinationSum](https://leetcode-cn.com/problems/k-closest-points-to-origin/)


```java
class Solution {
    class Node{
        int x;
        int y;
        double dist;
        Node(int x, int y){
            this.x = x;
            this.y = y;
            this.dist = Math.sqrt(Math.pow(x,2) + Math.pow(y,2));
        }
        
    }

    public int[][] kClosest(int[][] points, int k) {
        int[][] ans = new int[k][2];
        PriorityQueue<Node> priorityQueue = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                if (o1.dist > o2.dist){
                    return 1;
                }else if (o1.dist < o2.dist){
                    return -1;
                }else {
                    return 0;
                }
            }
        });
        for (int[] point : points) {
            priorityQueue.add(new Node(point[0], point[1]));
        }
        for (int i = 0; i < k && !priorityQueue.isEmpty(); i ++){
            Node node = priorityQueue.poll();
            ans[i][0] = node.x;
            ans[i][1] = node.y;
        }
        return ans;
    }
}
```
