---
title: leetcode-2150-Find All Lonely Numbers in the Array
date: 2022-06-11 20:20:52
summary: 找出数组中的所有孤独数字
categories: leetcode
tags: 

---
## 找出数组中的所有孤独数字
[leetcode-2150-Find All Lonely Numbers in the Array](https://leetcode.cn/problems/find-all-lonely-numbers-in-the-array/)





```java
class Solution {
    public List<Integer> findLonely(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        Arrays.sort(nums);
        if (nums.length == 1) {
            ans.add(nums[0]);
        } else {
            if (nums[0] < nums[1] - 1) {
                ans.add(nums[0]);
            }
            for (int i = 1; i < nums.length - 1; i++) {
                if (nums[i] > nums[i - 1] + 1 && nums[i] < nums[i + 1] - 1) {
                    ans.add(nums[i]);
                }
            }
            if (nums[nums.length - 1] > nums[nums.length - 2] + 1) {
                ans.add(nums[nums.length - 1]);
            }
        }
        return ans;
    }
}
```
