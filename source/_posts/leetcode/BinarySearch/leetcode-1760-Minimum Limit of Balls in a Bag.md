---
title: leetcode-1760-Minimum Limit of Balls in a Bag
date: 2022-05-12 22:50:52
summary: 袋子里最少数目的球
categories: leetcode
- 二分

---
## 搜索旋转排序数组
> 由于 值 互不相同
> 要么右侧递增，要么左侧递增
> 先检查数组右侧

[leetcode-1760-Minimum Limit of Balls in a Bag](https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/)


```java
public class MinimumSize {


    public int minimumSize(int[] A, int k) {
        int left = 1, right = 1_000_000_000;
        while (left < right) {
            int mid = (left + right) / 2, count = 0;
            for (int a : A) {
                count += (a - 1) / mid;
            }
            if (count > k) {
                left = mid + 1;
            }
            else {
                right = mid;
            }
        }
        return left;
    }


    public int minimumSize0(int[] nums, int maxOperations) {

        int l = 1;
        int r = 1_000_000_000;
        int ans = -1;
        while (l <= r){
            int mid = l + (r  - l >> 1);
            if (canSplit(nums, mid, maxOperations)){
                ans = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }

    private boolean canSplit(int[] nums, int count,int maxOperations){
        for (int num : nums){
            if (num > count){
                if (maxOperations <= 0){
                    return false;
                }
                count += (num - 1) / count;
            }
        }
        return maxOperations >= 0;
    }
}
```
