---
title: leetcode-908-SmallestRangeI
date: 2022-04-25 19:40:52
summary: 最小差值 I
categories: leetcode

---
## 最小差值 I
[leetcode-908-SmallestRangeI](https://leetcode-cn.com/problems/smallest-range-i/)


```java
class Solution {
    public int smallestRangeI(int[] nums, int k) {
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for (int num : nums) {
            max = Math.max(max, num);
            min = Math.min(min, num);
        }
        int a = Math.abs(max - min);
        int b = 2 * k;
        if (a > b){
            return a - b;
        }
        return 0;
    }
}
```
