---
title: leetcode-1818-Minimum Absolute Sum Difference
date: 2022-05-17 15:40:52
summary: 绝对差值和
categories: leetcode
- 二分

---
## 绝对差值和


[leetcode-1818-Minimum Absolute Sum Difference](https://leetcode.cn/problems/minimum-absolute-sum-difference/)


```java
public class MinAbsoluteSumDiff {

    public static void main(String[] args) {
        minAbsoluteSumDiff(new int[]{1,10,4,4,2,7}, new int[]{9,3,5,1,7,4});
    }


    static final int MOD = 1000000007;

    public static int minAbsoluteSumDiff(int[] nums1, int[] nums2) {

        // 所有的绝对差值和
        int abs = 0;
        int maxn = Integer.MIN_VALUE;
        int[] num3 = Arrays.copyOf(nums1, nums1.length);
        Arrays.sort(num3);
        for (int i = 0; i < nums1.length; i ++){

            int a = Math.abs(nums1[i] - nums2[i]);
            abs = (abs + Math.abs(a)) % MOD;

            int j = binarySearch(num3, nums2[i]);

            if (j < nums1.length) {
                maxn = Math.max(maxn, a - (num3[j] - nums2[i]));
            }
            if (j > 0) {
                maxn = Math.max(maxn, a - (nums2[i] - num3[j - 1]));
            }
        }
        return (abs - maxn + MOD) % MOD;
    }

    public static int binarySearch(int[] rec, int target) {
        int low = 0, high = rec.length - 1;
        if (rec[high] < target) {
            return high + 1;
        }
        while (low < high) {
            int mid = (high - low) / 2 + low;
            if (rec[mid] < target) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }

}
```
