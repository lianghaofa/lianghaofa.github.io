---
title: leetcode-540-Single Element in a Sorted Array
date: 2022-05-13 19:50:52
summary: 有序数组中的单一元素
categories: leetcode
- 二分

---
## 有序数组中的单一元素


[leetcode-540-Single Element in a Sorted Array](https://leetcode.cn/problems/single-element-in-a-sorted-array/)


```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int l = 0;
        int r = nums.length - 1;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (mid %  2 == 1){
                if (nums[mid - 1] == nums[mid]){
                    l = mid + 1;
                }else if (mid + 1 < nums.length && nums[mid + 1] == nums[mid] ){
                    r = mid - 1;
                }else {
                    return nums[mid];
                }
            }else {
                if (mid - 1 >= 0 && nums[mid - 1] == nums[mid] ){
                    r = mid - 1;
                }else if (mid + 1 < nums.length && nums[mid + 1] == nums[mid] ){
                    l = mid + 1;
                }else {
                    return nums[mid];
                }
            }
        }
        return -1;
    }
}
```
