---
title: leetcode-77-Combine
date: 2022-04-25 18:40:52
summary: 组合
categories: leetcode
tags:
- DFS   

---
## 组合
[leetcode-77-MySqrt](https://leetcode-cn.com/problems/combinations/)


```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        combineN( 1, n, k,  ans, new ArrayList<>());
        return ans;
    }
    
    private void combineN(int start, int end, int k, List<List<Integer>> ans, List<Integer> a){
        if (k == 0){
            ans.add(new ArrayList<>(a));
            return;
        }
        if (start == end && k > 1 || start + k - 1 > end){
            return;
        }
        a.add(start);
        combineN(start + 1, end,  k - 1,  ans,  a);
        a.remove(a.size() - 1);
        combineN(start + 1, end,  k,  ans,  a);
    }
}
```
