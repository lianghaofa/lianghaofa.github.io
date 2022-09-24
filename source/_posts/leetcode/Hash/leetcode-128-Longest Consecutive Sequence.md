---
title: leetcode-128-Longest Consecutive Sequence
date: 2022-04-29 19:40:52
summary: 最接近原点的 K 个点
categories: leetcode
tags:
- Hash   

---
## 最长连续序列
[leetcode-128-Longest Consecutive Sequence](https://leetcode.cn/problems/longest-consecutive-sequence/)

>  set 去重，同时快速查询
>  只能从开头的数字遍历

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }
        int ans = 0;
        // 从开头的数字遍历       set 而不是 nums ，如果是 nums ，开头的数字出现多次，会出现重复遍历。
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int s = num + 1;
                while (set.contains(s)){
                    s ++;
                }
                ans = Math.max(ans, s - num);
            }
        }
        return ans;
    }
}
```
