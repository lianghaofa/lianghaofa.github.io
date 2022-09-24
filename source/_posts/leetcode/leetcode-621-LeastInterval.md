---
title: leetcode-621-LeastInterval
date: 2022-04-27 19:40:52
summary: 任务调度器
categories: leetcode
tags:
- Queue   

---
## 组合
[leetcode-621-LeastInterval](https://leetcode-cn.com/problems/task-scheduler/)
> 任务可以以任意顺序执行

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
