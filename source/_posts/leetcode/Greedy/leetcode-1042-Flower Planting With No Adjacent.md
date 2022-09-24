---
title: leetcode-1042-Flower Planting With No Adjacent
date: 2022-06-07 19:10:52
summary: 不邻接植花
categories: leetcode
tags:
- 贪心   

---
## 不邻接植花
[leetcode-1042-Flower Planting With No Adjacent](https://leetcode.cn/problems/flower-planting-with-no-adjacent/)


### 贪心

```java
    public static int[] gardenNoAdj(int n, int[][] paths) {

        int[] ans = new int[n];
        Map<Integer, Set<Integer>> map = new HashMap<>();
        for (int i = 0; i < n; i ++){
            map.put(i, new HashSet<>());
        }
        for (int[] path : paths){
            int x = path[0] - 1;
            int y = path[1] - 1;
            map.get(x).add(y);
            map.get(y).add(x);
        }

        for (int i = 0; i < n; i ++){
            Set<Integer> set = map.get(i);
            int[] arr = new int[5];
            for (int num : set){
                if (ans[num] != 0){
                    arr[ans[num]] = 1;
                }
            }
            for (int j = arr.length - 1; j >= 1; j --){
                if (arr[j] == 0){
                    ans[i] = j;
                }
            }
        }
        return ans;
    }
```
