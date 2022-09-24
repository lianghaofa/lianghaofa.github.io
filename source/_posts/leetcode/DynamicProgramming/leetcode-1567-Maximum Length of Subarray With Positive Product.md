---
title: leetcode-1567-Maximum Length of Subarray With Positive Product
date: 2022-06-06 15:40:52
summary: 乘积为正数的最长子数组长度
categories: leetcode
tags:
- DP

---
## 乘积为正数的最长子数组长度
[leetcode-1567-Maximum Length of Subarray With Positive Product](https://leetcode.cn/problems/maximum-length-of-subarray-with-positive-product/)


#### 乘积为正数的最长子数组长度

```java
class Solution {
    public static int getMaxLen(int[] nums) {

        // 偶数个负数
        // 奇数个负数
        int negativeCount = 0;
        int ans = 0;
        int l = 0;
        boolean first = false;
        int firstIndex = -1;
        int lastIndex = -1;
        for (int r = 0;r <= nums.length; r ++){
            if (r == nums.length){
                if (nums[r - 1] != 0){
                    ans = Math.max(ans, getMaxLen(l, r - 1, negativeCount, firstIndex, lastIndex));
                }
            } else if (nums[r] == 0){
                // 偶数个负数
                ans = Math.max(ans, getMaxLen(l, r - 1, negativeCount, firstIndex, lastIndex));
                first = false;
                l = r + 1;
                negativeCount = 0;
            }else if (nums[r] < 0){
                if (!first){
                    firstIndex = r;
                    first = true;
                }
                lastIndex = r;
                negativeCount ++;
            }
        }
        return ans;
    }

    private static int getMaxLen(int l, int r, int negativeCount, int firstNegative, int lastNegative){
        if (negativeCount % 2 == 0){
            return (r - l + 1);
        }else {
            return Math.max((r - (firstNegative + 1) + 1), (lastNegative - 1 - l + 1));
        }
    }
}
```
