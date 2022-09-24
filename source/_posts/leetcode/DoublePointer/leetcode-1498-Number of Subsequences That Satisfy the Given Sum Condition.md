---
title: leetcode-1498-Number of Subsequences That Satisfy the Given Sum Condition
date: 2022-05-16 09:10:52
summary: 满足条件的子序列数目
categories: leetcode
tags:
- 双指针

---
## 满足条件的子序列数目
[leetcode-1498-Number of Subsequences That Satisfy the Given Sum Condition](https://leetcode.cn/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/)

> 排序
> 0 -> nums.length - 1, 累加所有的可能性

### 双指针

```java
class Solution {

    static final int P = 1000000007;
    
    public static int numSubseq(int[] nums, int target) {

        Arrays.sort(nums);
        int ans = 0;
        
        int r = nums.length;
        int n = nums.length, mod = (int)1e9 + 7;
        int[] pows = new int[n];
        pows[0] = 1;
        for (int i = 1 ; i < n ; ++i) {
            pows[i] = pows[i - 1] * 2 % mod;
        }
        // 以 i 开头的有多少个
        for (int i = 0; i < nums.length; i ++){
            while (r - 1 >= i && nums[r - 1] + nums[i] > target){
                r --;
            }
            if (r - 1 < i){
                break;
            }
            int count = (r - 1) - (i + 1) + 1;
            ans += pows[count];
            ans %= P;
        }
        return ans;
    }
}
```