---
title: leetcode-1968-Array With Elements Not Equal to Average of Neighbors
date: 2022-06-10 15:20:52
summary: 构造元素不等于两相邻元素平均值的数组
categories: leetcode
tags: 

---
## 构造元素不等于两相邻元素平均值的数组
[leetcode-1968-Array With Elements Not Equal to Average of Neighbors](https://leetcode.cn/problems/array-with-elements-not-equal-to-average-of-neighbors/)



```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int[] ans = new int[nums.length];

        Arrays.sort(nums);
        int index = 0;
        int l = 0;
        int r = nums.length - 1;
        while (l < r){
            ans[index ++] = nums[l ++];
            ans[index ++] = nums[r --];
        }
        if (l == r){
            ans[index] = nums[l];
        }
        return ans;
    }
}
```
