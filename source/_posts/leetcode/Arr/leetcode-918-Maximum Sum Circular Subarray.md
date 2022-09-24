---
title: leetcode-918-Maximum Sum Circular Subarray
date: 2022-06-02 14:20:52
summary: 复写零
categories: leetcode
tags: 

---
## 环形子数组的最大和
[leetcode-918-Maximum Sum Circular Subarray](https://leetcode.cn/problems/maximum-sum-circular-subarray/)



![环形子数组的最大和](/medias/LeetCode/1654151017.jpg)

```java
class Solution {
    public int maxSubarraySumCircular(int[] A) {
        int total = 0, maxSum = A[0], curMax = 0, minSum = A[0], curMin = 0;
        for (int a : A) {
            curMax = Math.max(curMax + a, a);
            maxSum = Math.max(maxSum, curMax);
            curMin = Math.min(curMin + a, a);
            minSum = Math.min(minSum, curMin);
            total += a;
        }
        return maxSum > 0 ? Math.max(maxSum, total - minSum) : maxSum;
    }

}
```
