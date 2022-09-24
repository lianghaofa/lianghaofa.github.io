---
title: leetcode-915-Partition Array into Disjoint Intervals
date: 2022-06-11 20:20:52
summary: 分割数组
categories: leetcode
tags: 

---
## 分割数组
[leetcode-915-Partition Array into Disjoint Intervals](https://leetcode.cn/problems/partition-array-into-disjoint-intervals/)





```java
class Solution {
    public int partitionDisjoint(int[] nums) {
        int max = nums[0];
        int leftMax = max;
        int ans = 1;
        for (int i = 1; i < nums.length - 1; i ++){
            if (max > nums[i]){
                ans = i + 1;
                max = leftMax;
            }
            leftMax = Math.max(leftMax, nums[i]);
        }
        return ans;
    }
}
```
