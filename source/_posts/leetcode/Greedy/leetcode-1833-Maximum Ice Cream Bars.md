---
title: leetcode-1833-Maximum Ice Cream Bars
date: 2022-06-18 11:10:52
summary: 雪糕的最大数量
categories: leetcode
tags:
- 贪心   

---
## 雪糕的最大数量
[leetcode-1833-Maximum Ice Cream Bars](https://leetcode.cn/problems/maximum-ice-cream-bars/)


### 贪心

```java
class Solution {
    public static int maxIceCream(int[] costs, int coins) {

        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });

        int cost = 0;
        for (int value : costs) {
            if (value > coins) {
                continue;
            }
            if (priorityQueue.isEmpty()) {
                priorityQueue.add(value);
                cost = value;
            } else {
                if (cost + value <= coins) {
                    cost += value;
                    priorityQueue.add(value);
                } else if (priorityQueue.peek() > value) {
                    Integer val = priorityQueue.poll();
                    priorityQueue.add(value);
                    cost -= val - value;
                }
            }
        }
        return priorityQueue.size();
    }
}
```
