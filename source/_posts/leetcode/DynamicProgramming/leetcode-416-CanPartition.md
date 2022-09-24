---
title: leetcode-416-CanPartition
date: 2022-04-27 14:40:52
summary: 分割等和子集
categories: leetcode
tags:
- DP

---
## 分割等和子集
[leetcode-416-CanPartition](https://leetcode-cn.com/problems/partition-equal-subset-sum/)


```java
public class CanPartition {

    public static void main(String[] args) {
        canPartition(new int[]{14,9,8,4,3,2});
    }

    public static boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (sum % 2 == 1){
            return false;
        }
        int m = nums.length;
        int n = sum / 2 + 1;
        boolean[][] dp = new boolean[m][n];
        dp[0][0] = true;
        if (nums[0] < n){
            dp[0][nums[0]] = true;
        }
        if (dp[0][dp[0].length - 1]){
            return true;
        }
        for (int i = 1; i < m; i ++){
            for (int j = 0; j < n; j ++){
                if (dp[i - 1][j]){
                    dp[i][j] = true;
                }
                if (dp[i - 1][j] && j + nums[i] < n){
                    dp[i][j + nums[i]] = true;
                }
            }
            if (dp[i][n - 1]){
                return true;
            }
        }
        return false;
    }
}
```

