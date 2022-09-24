---
title: leetcode-34-Find First and Last Position of Element in Sorted Array
date: 2022-04-30 23:50:52
summary: 在排序数组中查找元素的第一个和最后一个位置
categories: leetcode
- 二分

---
## 在排序数组中查找元素的第一个和最后一个位置
> 利用 ans 变量

[leetcode-34-Find First and Last Position of Element in Sorted Array](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```java
    public int[] searchRange(int[] nums, int target) {
        int l = searchLeft(nums, target);
        int r = searchRight(nums, target);
        return new int[]{l , r};
    }

    private int searchRight(int[] nums, int target) {
        int ans = -1;
        int l = 0;
        int r = nums.length - 1;
        while (l <= r){
            int mid = l + ( r - l >> 1);
            if (nums[mid] == target){
                ans = mid;
                l = mid + 1;
            }else if (nums[mid] > target){
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }

    private int searchLeft(int[] nums, int target) {
        int ans = -1;
        int l = 0;
        int r = nums.length - 1;
        while (l <= r){
            int mid = l + ( r - l >> 1);
            if (nums[mid] == target){
                ans = mid;
                r = mid - 1;
            }else if (nums[mid] > target){
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }
```
