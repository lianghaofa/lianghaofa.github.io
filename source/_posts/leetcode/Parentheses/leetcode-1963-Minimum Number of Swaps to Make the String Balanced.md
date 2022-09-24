---
title: leetcode-1963-Minimum Number of Swaps to Make the String Balanced
date: 2022-06-20 20:20:52
summary: 使字符串平衡的最小交换次数
categories: leetcode
tags:
 

---
## 使括号有效的最少添加
[leetcode-1963-Minimum Number of Swaps to Make the String Balanced](https://leetcode.cn/problems/minimum-number-of-swaps-to-make-the-string-balanced/)


```java
class Solution {
    public int minSwaps(String s) {

        // 第一个违法的 ]

        // 倒数第一个违法的 [
        int l = 0;
        int l0 = 0;
        int l1 = 0;

        int r = s.length() - 1;
        int r0 = 0;
        int r1 = 0;
        int ans = 0;
        while (l <= r){

            while (l < s.length() && l1 <= l0){
                if (s.charAt(l) == '['){
                    l0 ++;
                }else {
                    l1 ++;
                }
                l ++;
            }

            while (r >= 0 && r0 <= r1){
                if (s.charAt(r) == '['){
                    r0 ++;
                }else {
                    r1 ++;
                }
                r --;
            }

            if (l <= r + 1){
                ans ++;
                l0 ++;
                l1 --;

                r0 --;
                r1 ++;
            }


        }

        return ans;
    }
}
```
