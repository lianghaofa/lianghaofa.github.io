---
title: leetcode-1834-Single-Threaded CPU
date: 2022-06-12 19:40:52
summary: 单线程 CPU
categories: leetcode
tags:
- 堆   

---
## 单线程 CPU
[leetcode-1834-Single-Threaded CPU](https://leetcode.cn/problems/single-threaded-cpu/)


```java
class Solution {
    static class Node{
        int index;
        int start;
        int time;
        Node(int index, int start, int time){
            this.index = index;
            this.start = start;
            this.time = time;
        }

    }

    public static int[] getOrder(int[][] tasks) {

        // timeQueue 时间排序，把符合时间的放到 taskQueue


        
        Node[] nodes = new Node[tasks.length];
        
        for (int i = 0; i < tasks.length; i ++){
            nodes[i] = new Node(i, tasks[i][0], tasks[i][1]);
        }

        Arrays.sort(nodes, new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                return o1.start - o2.start;
            }
        });



        PriorityQueue<Node>  taskQueue = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                if (o1.time == o2.time){
                    return o1.index - o2.index;
                }
                return o1.time - o2.time;
            }
        });

        int cur = 0;
        int i = 0;
        int j = 0;
        int[] ans = new int[tasks.length];
        while (j < nodes.length || !taskQueue.isEmpty()){
            while (j < nodes.length && cur >= nodes[j].start){


                taskQueue.add(nodes[j]);
                j ++;
            }


            if (!taskQueue.isEmpty()){
                Node node = taskQueue.poll();
                ans[i ++] = node.index;
                if (cur >= node.start){
                    cur = cur + node.time;
                } else {
                    cur = node.start + node.time;
                }
            }else {
                if (j < tasks.length){
                    cur = nodes[j].start;
                }
            }
        }
        return ans;
    }
}
```
