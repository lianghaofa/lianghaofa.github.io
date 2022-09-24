---
title: leetcode-528-RandomPickwithWeight
date: 2022-04-23 13:40:52
summary: 按权重随机选择
categories: leetcode
tags:
- 二分
- random

---
## MinCut
[leetcode-528-RandomPickwithWeight](https://leetcode-cn.com/problems/random-pick-with-weight/)



```java
public class Solution {

    int[] ans;
    int sum = 0;
    public Solution(int[] w) {
        ans = new int[w.length];
        int sum = 0;
        for (int i = 0;i < ans.length; i ++){
            sum += w[i];
            ans[i] = sum;
        }
        this.sum = sum;
    }

    public int pickIndex() {
        int s = (int) (Math.random() * sum) + 1;
        int l = 0;
        int r = ans.length - 1;
        int res = ans.length - 1;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (ans[mid] == s){
                return mid;
            }else if (ans[mid] > s){
                res = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return res;
    }
}
```