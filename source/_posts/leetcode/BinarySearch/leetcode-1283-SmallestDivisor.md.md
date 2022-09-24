---
title: leetcode-1283-Find the Smallest Divisor Given a Threshold
date: 2022-05-06 09:40:52
summary: 使结果不超过阈值的最小除数
categories: leetcode
- 二分

---
## 使结果不超过阈值的最小除数


[leetcode-1283-Find the Smallest Divisor Given a Threshold](https://leetcode-cn.com/problems/find-the-smallest-divisor-given-a-threshold/)


```java
class Solution {
    public int smallestDivisor(int[] nums, int threshold) {
        int l = 1;
        int r = 1_000_000;
        int ans = 1;
        while (l <= r){
            int mid = l + ( r - l >> 1);
            if (div(nums, mid, threshold)){
                ans = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }


    private boolean div(int[] nums, int b, int threshold){
        int ans = 0;
        for (int num : nums) {
            ans += (num - 1) / b + 1;
        }
        return ans <= threshold;
    }
}
```
