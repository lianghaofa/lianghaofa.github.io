---
title: leetcode-444-Is Graph Bipartite
date: 2022-05-08 15:40:52
summary: 判断二分图
categories: leetcode
tags:
- 拓扑排序   


---
## 重建序列
[leetcode-444-Is Graph Bipartite](https://leetcode.cn/problems/sequence-reconstruction/)


### 拓扑排序 


```java
class Solution {
    public static boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {

        int[] inDegree = new int[10001];
        HashMap<Integer, Set<Integer>> hashMap = new HashMap<>();
        Set<Integer> beginList = new HashSet<>();
        for (List<Integer> list : seqs) {
            int s = list.get(0);
            if (s > 10000) {
                return false;
            }
            beginList.add(s);
            for (int j = 0; j < list.size() - 1; j++) {
                int start = list.get(j);
                int end = list.get(j + 1);
                if (start > 10000 || start <= 0 || end <= 0 || end > 10000) {
                    return false;
                }
                Set<Integer> set = hashMap.get(start);
                if (set == null) {
                    set = new HashSet<>();
                    set.add(end);
                    inDegree[end]++;
                } else if (!set.contains(end)) {
                    set.add(end);
                    inDegree[end]++;
                }
                hashMap.put(start, set);
            }
        }

        Queue<Integer> queue = new LinkedList<>();
        for (Integer s : beginList){
            if (inDegree[s] == 0){
                queue.add(s);
            }
        }
        int count = 0;
        while (!queue.isEmpty()){
            Integer q = queue.poll();
            if (queue.size() >= 1 || org[count ++] != q){
                return false;
            }
            Set<Integer> child = hashMap.get(q);
            if (child != null){
                for (Integer s : child){
                    inDegree[s] --;
                    if (inDegree[s] == 0){
                        queue.add(s);
                    }
                }
            }
        }

        for (int value : inDegree) {
            if (value > 0) {
                return false;
            }
        }
        return count == org.length;
    }
}
```
