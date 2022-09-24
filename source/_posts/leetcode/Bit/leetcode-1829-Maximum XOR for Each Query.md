---
title: leetcode-1829-Maximum XOR for Each Query
date: 2022-04-25 19:40:52
summary: 每个查询的最大异或值
categories: leetcode
tags:
- Bits   

---
## 每个查询的最大异或值
[leetcode-1829-Maximum XOR for Each Query](https://leetcode.cn/problems/maximum-xor-for-each-query/)


```java
class Solution {
    public int[] getMaximumXor(int[] nums, int maximumBit) {
        int[] ans = new int[nums.length];
        int b = (1 << maximumBit) - 1;
        int c = b & nums[0];
        ans[nums.length - 1] = c ^ b;
        for (int i = 1; i < nums.length; i ++){
            c ^= nums[i];
            ans[nums.length - i - 1] = c ^ b;
        }
        return ans;
    }
}
```
