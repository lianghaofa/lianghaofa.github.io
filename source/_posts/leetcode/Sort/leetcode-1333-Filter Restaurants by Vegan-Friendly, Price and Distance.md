---
title: leetcode-1333-Filter Restaurants by Vegan-Friendly, Price and Distance
date: 2022-06-28 15:40:52
summary: 餐厅过滤器
categories: leetcode
tags:
-   

---
## 餐厅过滤器
[leetcode-1333-Filter Restaurants by Vegan-Friendly, Price and Distance](https://leetcode.cn/problems/filter-restaurants-by-vegan-friendly-price-and-distance/)


```java
public class FilterRestaurants {

    class Node{
        int id;
        int rating;
        Node(int id, int rating){
            this.id = id;
            this.rating = rating;
        }

    }

    public List<Integer> filterRestaurants(int[][] restaurants, int veganFriendly, int maxPrice, int maxDistance) {
        List<Integer> ans = new ArrayList<>();

        // 大根堆
        PriorityQueue<Node> priorityQueue = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                if (o1.rating == o2.rating){
                    return o2.id - o1.id;
                }
                return o2.rating - o1.rating;
            }
        });

        for (int[] restaurant : restaurants) {
            if (veganFriendly == 1 && restaurant[2] == 0 || restaurant[3] > maxPrice || restaurant[4] > maxDistance) {
                continue;
            }
            priorityQueue.add(new Node(restaurant[0], restaurant[1]));
        }

        while (!priorityQueue.isEmpty()){
            ans.add(priorityQueue.poll().id);
        }

        return ans;
    }
}
```
