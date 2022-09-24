---
title: leetcode-684-Redundant Connection
date: 2022-05-11 09:10:52
summary: 冗余连接
categories: leetcode
tags:
- 并查集


---
## 冗余连接
[leetcode-684-Redundant Connection](https://leetcode.cn/problems/redundant-connection/)


### 并查集 


```java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {

        int[] nodes = new int[1001];
        for (int i = 0; i < nodes.length; i ++){
            nodes[i] = i;
        }
        for (int[] edge : edges) {
            int s = edge[0];
            int e = edge[1];
            int sParent = findParent(nodes, s);
            int eParent = findParent(nodes, e);
            if (sParent == eParent) {
                return edge;
            } else {
                nodes[sParent] = eParent;
            }
        }
        return new int[]{};
    }
    
    private int findParent(int[] nodes, int cur){
        if (nodes[cur] != cur){
            nodes[cur] = findParent(nodes, nodes[cur]);
        }
        return nodes[cur];
    }
}
```

