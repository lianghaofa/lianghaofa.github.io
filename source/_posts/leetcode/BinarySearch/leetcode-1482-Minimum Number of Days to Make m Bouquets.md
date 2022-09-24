---
title: leetcode-1482-Minimum Number of Days to Make m Bouquets
date: 2022-05-16 18:30:52
summary: 搜索旋转排序数组
categories: leetcode
- 二分

---
## 制作 m 束花所需的最少天数
> 指定 范围 l = 1, r = 最大值
> 暴力尝试


[leetcode-1482-Minimum Number of Days to Make m Bouquets](https://leetcode.cn/problems/minimum-number-of-days-to-make-m-bouquets/)

```java
class Solution {
    public static int minDays(int[] bloomDay, int m, int k) {
        int l = 1, r = Integer.MIN_VALUE;
        int num = m * k;
        int ans = -1;
        for (int value : bloomDay) {
            r = Math.max(r, value);
        }
        if (num > bloomDay.length){
            return ans;
        }
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (canWork(bloomDay, mid, m, k)){
                ans = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }

    private static boolean canWork(int[] bloomDay, int mid, int m, int k) {
        for (int i = 0; i < bloomDay.length && m > 0;){
            int c = 0, j = 0;
            for (; j < k && i + j < bloomDay.length; j ++){
                if (bloomDay[i + j] <= mid){
                    c ++;
                }else {
                    break;
                }
            }
            if (c == k){
                m --;
                i += k;
            }else {
                i += j + 1;
            }
        }
        return  m <= 0;
    }
}
```
