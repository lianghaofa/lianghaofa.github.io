---
title: leetcode-547-sequence-Number of Provinces
date: 2022-05-10 12:10:52
summary: 判断二分图
categories: leetcode
tags:
- DP
- 并查集


---
## 省份数量
[leetcode-547-sequence-Number of Provinces](https://leetcode.cn/problems/number-of-provinces/)


### 并查集 


```java
class Solution {
    
    public int findCircleNum(int[][] isConnected) {

        int[] cities = new int[isConnected.length];

        for (int i = 0; i < cities.length; i ++){
            cities[i] = i;
        }
        for (int i = 0; i < isConnected.length; i ++){
            int p = findParent(cities, i);
            for (int j = i + 1; j < isConnected[i].length; j ++){
                if (isConnected[i][j] == 1){
                    cities[findParent(cities, j)] = p;
                }
            }
        }
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < cities.length; i ++){
            findParent(cities, i);
            set.add(cities[i]);
        }

        return set.size();
    }


    private int findParent(int[] cities, int cur){
        if (cities[cur] == cur){
            return cur;
        }
        int parent = findParent(cities, cities[cur]);
        cities[cur] = parent;
        return parent;
    }
}
```

### DP
```java
class Solution {

    public int findCircleNum(int[][] isConnected) {
        int[] cities = new int[isConnected.length];
        int ans = 0;
        for (int i = 0;i < cities.length;i ++){
            if (cities[i] >= 0){
                cities[i] = -1;
                dfs(isConnected,cities,i);
                ans ++;
            }
        }
        return ans;
    }

    private void dfs(int[][] isConnected,int[] cities,int index){
        for (int i = 0;i < isConnected[index].length;i ++){
            if (cities[i] >= 0 && isConnected[index][i] == 1){
                cities[i] = -1;
                dfs(isConnected,cities,i);
            }
        }
    }
}
```
