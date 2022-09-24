---
title: leetcode-785-Is Graph Bipartite
date: 2022-05-08 15:40:52
summary: 判断二分图
categories: leetcode
tags:
- BFS   
- DFS

---
## 判断二分图
[leetcode-785-Is Graph Bipartite](https://leetcode-cn.com/problems/01-matrix/)


### BFS 
> 可能不是连通图
> 当存在多个连通图，需要把每个连通图都拆分成功

```java
class Solution {
    public boolean isBipartite(int[][] graph) {

        int[] nodes = new int[graph.length];
        for (int i = 0; i < graph.length; i ++){
            
            if (graph[i].length > 0 && nodes[i] == 0){
                Queue<Integer> queue = new LinkedList<>();
                nodes[i] = 101;
                queue.add(i);
                int count = queue.size();
                while (!queue.isEmpty()){
                    while (count > 0){
                        int node = queue.poll();
                        int group = nodes[node];
                        count --;
                        for (int index = 0; index < graph[node].length; index ++){
                            int curNode = graph[node][index];
                            if (nodes[curNode] == group){
                                return false;
                            }
                            if (nodes[curNode] == 0){
                                queue.add(graph[node][index]);
                            }
                            nodes[curNode] = -group;
                        }
                    }
                    count = queue.size();
                }
                
            }
        }
        return true;
    }
}
```
