---
title: leetcode-153-Find Minimum in Rotated Sorted Array
date: 2022-04-30 23:50:52
summary: 寻找旋转排序数组中的最小值
categories: leetcode
- 二分

---
## 寻找旋转排序数组中的最小值
> 由于 值 互不相同
> 要么右侧递增，要么左侧递增
> 右侧递增，ans = nums[mid], 向左侧继续寻找
> 左侧递增，ans = nums[l], 向右侧继续寻找

[leetcode-153-Find Minimum in Rotated Sorted Array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)


```java
    public int findMin(int[] nums) {
        int ans = Integer.MAX_VALUE;
        int l = 0;
        int r = nums.length - 1;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (nums[mid] < nums[r]){
                ans = Math.min(ans, nums[mid]);
                r = mid - 1;
            }else {
                ans = Math.min(ans, nums[l]);
                l = mid + 1;
            }
        }
        return ans;
    }
```
