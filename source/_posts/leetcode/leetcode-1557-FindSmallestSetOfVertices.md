---
title: leetcode-1557-FindSmallestSetOfVertices
date: 2022-04-21 19:56:52
summary: 可以到达所有点的最少点数目
categories: leetcode
tags:
- 图

---
## LargestPerimeter
[leetcode-1557-FindSmallestSetOfVertices](https://leetcode-cn.com/problems/minimum-number-of-vertices-to-reach-all-nodes/)

``` java
    public List<Integer> findSmallestSetOfVertices(int n, List<List<Integer>> edges) {
        List<Integer> ans = new ArrayList<>();
        int[] in = new int[n];
        for (List<Integer> list : edges) {
            in[list.get(1)]++;
        }
        for (int i = 0; i < in.length; i ++){
            if (in[i] == 0){
                ans.add(i);
            }
        }
        return ans;
    }
```