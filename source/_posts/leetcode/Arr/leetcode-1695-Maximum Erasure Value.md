---
title: leetcode-1695-Maximum Erasure Value
date: 2022-06-09 14:20:52
summary: 删除子数组的最大得分
categories: leetcode
tags: 

---
## 删除子数组的最大得分
[leetcode-1695-Maximum Erasure Value](https://leetcode.cn/problems/maximum-erasure-value/)



```java
class Solution {
    public int maximumUniqueSubarray(int[] nums) {

        int l = 0;
        int r = 0;
        int ans = 0;
        int sum = 0;
        Set<Integer> set = new HashSet<>();
        while (r < nums.length){

            if (set.contains(nums[r])){

                // 把 nums[r] 移除

                while (nums[l] != nums[r]){
                    set. remove(nums[l]);
                    sum -= nums[l];
                    l ++;
                }
                sum -= nums[l];
                set. remove(nums[l]);
                l ++;

            }
            sum += nums[r];
            set.add(nums[r ++]);

            ans = Math.max(sum, ans);
        }

        return ans;
    }
}
```
