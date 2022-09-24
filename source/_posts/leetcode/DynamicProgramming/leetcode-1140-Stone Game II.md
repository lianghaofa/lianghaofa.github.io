---
title: leetcode-1140-Stone Game II
date: 2022-05-01 21:05:52
summary: 打家劫舍 II
categories: leetcode
tags:
- DP

---
##  石子游戏 II
> 抢N，就不能抢0
> 抢0，就不能抢N

[leetcode-1140-Stone Game II](https://leetcode.cn/problems/stone-game-ii/)

```java
    public int rob(int[] nums) {
        if (nums.length == 0){
            return 0;
        }
        if (nums.length == 1){
            return nums[0];
        }
        return Math.max(rob(nums, 0, nums.length - 2), rob(nums, 1, nums.length - 1));
    }

    public int rob(int[] nums, int start, int end) {
        if (start > end){
            return 0;
        }
        int rob0 = 0, rob1 = nums[start];
        for (int i = start + 1; i <= end; i ++){
            int r = Math.max(rob0, rob1);
            rob1 = rob0 + nums[i];
            rob0 = r;
        }
        return Math.max(rob0, rob1);
    }
```

