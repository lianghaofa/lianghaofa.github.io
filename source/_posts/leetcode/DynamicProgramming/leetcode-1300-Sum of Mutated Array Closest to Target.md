---
title: leetcode-1300-Sum of Mutated Array Closest to Target
date: 2022-05-23 18:40:52
summary: 转变数组后最接近目标值的数组和
categories: leetcode
tags:
- DP

---
## 转变数组后最接近目标值的数组和
[leetcode-1300-Sum of Mutated Array Closest to Target](https://leetcode.cn/problems/sum-of-mutated-array-closest-to-target/)


```java
class Solution {
    public static int findBestValue(int[] arr, int target) {

        int max = Integer.MIN_VALUE;
        for (int m : arr) {
            max = Math.max(m, max);
        }
        if (target >= arr.length * max){
            return max;
        }
        int l = 0;
        int r = max;
        int ans = -1;
        int abs = Integer.MAX_VALUE;
        while (l <= r){
            int mid = l + (r - l >> 1);
            int res = getSum(arr, mid);
            if (res == target){
                abs = 0;
                r = mid - 1;
                ans = mid;
            } else if (res > target){
                if (Math.abs(res - target) < abs ){
                    abs = Math.abs(res - target);
                    ans = mid;
                }
                r = mid - 1;
            }else {
                if (Math.abs(res - target) <= abs ){
                    abs = Math.abs(res - target);
                    ans = mid;
                }
                l = mid + 1;
            }
        }

        return ans;
    }

    private static   int getSum(int[] arr, int mid) {
        int sum = 0;
        for (int value : arr) {
            sum += Math.min(value, mid);
        }
        return sum;
    }
}
```
