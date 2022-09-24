---
title: leetcode-976-LargestPerimeter
date: 2022-04-20 19:56:52
summary: 最大周长
categories: leetcode
tags:
- 排序
- 贪心
---
## LargestPerimeter
[leetcode-976-LargestPerimeter](https://leetcode-cn.com/problems/largest-perimeter-triangle/)
排序
``` java
class Solution {
    public int largestPerimeter(int[] nums) {
        Arrays.sort(nums);
        for (int i = nums.length - 1; i >= 2; i --){
            if (nums[i] < nums[i - 1] + nums[i - 2]){
                return nums[i] + nums[i - 1] + nums[i - 2];
            }
        }
        return 0;
    }
}
```