---
title: leetcode-1218-Longest Arithmetic Subsequence of Given Difference
date: 2022-06-14 21:40:52
summary: 最长定差子序列
categories: leetcode
tags:
- DP

---
## 最长定差子序列
[leetcode-1218-Longest Arithmetic Subsequence of Given Difference](https://leetcode.cn/problems/longest-arithmetic-subsequence-of-given-difference/)


#### 最长定差子序列

```java
class Solution {
    public int longestSubsequence(int[] arr, int difference) {
        HashMap<Integer, Integer> hashMap = new HashMap<>();
        hashMap.put(arr[arr.length - 1], 1);
        int ans = 1;
        for (int i = arr.length - 2; i >= 0; i --){
            Integer val = hashMap.get(arr[i] + difference);
            if (val == null){
                hashMap.put(arr[i], 1);
            }else {
                hashMap.put(arr[i], val + 1);
                ans = Math.max(ans, val + 1);
            }
        }
        return ans;
    }
}
```
