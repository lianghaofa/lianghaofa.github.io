---
title: leetcode-1894-Find the Student that Will Replace the Chalk
date: 2022-05-13 21:50:52
summary: 找到需要补充粉笔的学生编号
categories: leetcode
- 二分

---
## 找到需要补充粉笔的学生编号


[leetcode-1894-Find the Student that Will Replace the Chalk](https://leetcode.cn/problems/find-the-student-that-will-replace-the-chalk/)


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
