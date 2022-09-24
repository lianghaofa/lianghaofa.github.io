---
title: leetcode-2102-Sequentially Ordinal Rank Tracker
date: 2022-04-29 19:40:52
summary: 序列顺序查询
categories: leetcode
tags:
- 堆   

---
## 序列顺序查询
[leetcode-2102-Sequentially Ordinal Rank Tracker](https://leetcode.cn/problems/sequentially-ordinal-rank-tracker/)

> 维护两个堆

```java
class SORTracker {

    class Node{
        String name;
        int score;
        Node(String name, int score){
            this.name = name;
            this.score = score;
        }
    }

    int i;
    PriorityQueue<Node> maxHeap;
    PriorityQueue<Node> minHeap;

    public SORTracker() {
        this.i = 0;
        this.maxHeap = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                if (o1.score == o2.score){
                    return o1.name.compareTo(o2.name);
                }
                return o2.score - o1.score;
            }
        });
        this.minHeap = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                if (o1.score == o2.score){
                    return o2.name.compareTo(o1.name);
                }
                return o1.score - o2.score;
            }
        });
    }

    public void add(String name, int score) {
        if (minHeap.isEmpty()){
            maxHeap.add(new Node(name, score));
        }else {
            Node node = minHeap.peek();
            if (node.score < score || (node.score == score && node.name.compareTo(name) >= 0)){
                Node n = minHeap.poll();
                maxHeap.add(n);
                minHeap.add(new Node(name, score));
            }else {
                maxHeap.add(new Node(name, score));
            }
        }
    }

    public String get() {
        while (minHeap.size() <= i){
            Node node = maxHeap.poll();
            minHeap.add(node);
        }
        i ++;
        return minHeap.peek().name;
    }
}
```
