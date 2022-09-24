---
title: leetcode-33-Search in Rotated Sorted Array
date: 2022-04-30 19:50:52
summary: 搜索旋转排序数组
categories: leetcode
- 二分

---
## 搜索旋转排序数组
> 由于 值 互不相同
> 要么右侧递增，要么左侧递增
> 先检查数组右侧

[leetcode-33-Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)


```java
public class Search {

    public int search(int[] nums, int target) {
        int l = 0;
        int r = nums.length - 1;
        while (l <= r){
            int mid = l + (r - l >> 1);
            //nums[mid] < nums[r];
            if (nums[mid] < nums[r]){
                if (nums[mid] == target){
                    return mid;
                }else if (nums[r] == target){
                    return r;
                }else if (target > nums[mid] && nums[r] > target){
                    l = mid + 1;
                }else {
                    r = mid - 1;
                }
            }else {
                if (nums[mid] == target){
                    return mid;
                }else if (nums[l] == target){
                    return l;
                }else if (nums[l] < target && target < nums[mid]){
                    r = mid - 1;
                }else {
                    l = mid + 1;
                }
            }
        }
        return -1;
    }
}
```
