---
title: leetcode-491-Increasing Subsequences
date: 2022-06-14 15:40:52
summary: 递增子序列
categories: leetcode
tags: 
- DFS

---
## 递增子序列
[leetcode-491-Increasing Subsequences](https://leetcode.cn/problems/increasing-subsequences/)


### DFS


```java
class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {

        List<List<Integer>> ans = new ArrayList<>();
        
        dfs(0, Integer.MIN_VALUE, nums, new ArrayList<>(), ans);

        return ans;
    }

    public void dfs(int cur, int last, int[] nums, List<Integer> temp, List<List<Integer>> ans) {
        if (cur == nums.length) {
            if (temp.size() >= 2) {
                ans.add(new ArrayList<Integer>(temp));
            }
            return;
        }
        if (nums[cur] >= last) {
            temp.add(nums[cur]);
            dfs(cur + 1, nums[cur], nums, temp, ans);
            temp.remove(temp.size() - 1);
        }
        if (nums[cur] != last) {
            dfs(cur + 1, last, nums, temp,  ans);
        }
    }
}
```
