---
title: leetcode-2149-Rearrange Array Elements by Sign
date: 2022-06-10 14:20:52
summary: 按符号重排数组
categories: leetcode
tags: 

---
## 按符号重排数组
[leetcode-2149-Rearrange Array Elements by Sign](https://leetcode.cn/problems/rearrange-array-elements-by-sign/)




```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int[] ans = new int[nums.length];
        int p = 0;
        int n = 1;

        for (int num : nums) {
            if (num > 0) {
                ans[p] = num;
                p += 2;
            } else {
                ans[n] = num;
                n += 2;
            }
        }

        return ans;
    }
}
```
