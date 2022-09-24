---
title: leetcode-797-AllPathsSourceTarget
date: 2022-05-08 11:40:52
summary: 所有可能的路径
categories: leetcode
tags:
- DFS   

---
## 所有可能的路径
[leetcode-797-AllPathsSourceTarget](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

> 有向无环图（DAG）

```java

class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {

        List<List<Integer>> lists = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        list.add(0);
        dfs(graph, lists,list, 0);
        return lists;
    }

    private void dfs(int[][] graph,List<List<Integer>> lists,List<Integer> list,int row){
        if (graph.length - 1 == row){
            lists.add(new ArrayList<>(list));
            return;
        }
        for (int i = 0;i < graph[row].length;i ++) {
            list.add(graph[row][i]);
            dfs(graph, lists, list, graph[row][i]);
            list.remove(list.size() - 1);
        }
    }
}
```
