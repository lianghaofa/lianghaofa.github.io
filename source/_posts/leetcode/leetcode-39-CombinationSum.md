---
title: leetcode-39-CombinationSum
date: 2022-04-25 19:40:52
summary: 组合总和
categories: leetcode
tags:
- DFS   

---
## 组合
[leetcode-39-CombinationSum](https://leetcode-cn.com/problems/combination-sum/)


```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        combineSum(candidates, target, 0, 0,  ans, new ArrayList<>());
        return ans;
    }

    private void combineSum(int[] candidates, int target, int start, int sum, List<List<Integer>> ans, List<Integer> a) {
        if (sum > target || start >= candidates.length){
            return;
        }
        if (sum == target){
            ans.add(new ArrayList<>(a));
            return;
        }
        a.add(candidates[start]);
        combineSum(candidates, target, start, sum + candidates[start], ans, a);
        a.remove(a.size() - 1);
        combineSum(candidates, target, start + 1, sum , ans, a);
    }
}
```
