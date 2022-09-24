---
title: leetcode-1319-Number of Operations to Make Network Connected
date: 2022-05-26 21:40:52
summary: 单词接龙
categories: leetcode
tags:
- 并查集   

---
## 连通网络的操作次数
[leetcode-1319-Number of Operations to Make Network Connected](https://leetcode.cn/problems/number-of-operations-to-make-network-connected/)

> 并查集

```java
public class MakeConnected {

    public int makeConnected(int n, int[][] connections) {
        int[] set = new int[n];
        for (int i = 0; i < set.length; i ++){
            set[i] = i;
        }
        // 多余电缆的个数
        int M = 0;
        for (int[] connection : connections) {
            int node1 = connection[0];
            int node2 = connection[1];
            int node1Parent = findParent(set, node1);
            int node2Parent = findParent(set, node2);
            if (node1Parent == node2Parent) {
                M++;
            } else {
                set[node1Parent] = node2Parent;
            }
        }
        // 集合个数
        int N = 0;
        for (int i = 0; i < set.length; i ++){
            findParent(set, i);
            if (set[i] == i){
                N ++;
            }
        }
        return M >=(N - 1) ? (N - 1) : -1 ;
    }

    private int findParent(int[] set, int cur){
        if (set[cur] == cur){
            return cur;
        }
        int parent = findParent(set, set[cur]);
        set[cur] = parent;
        return parent;
    }

}
```
