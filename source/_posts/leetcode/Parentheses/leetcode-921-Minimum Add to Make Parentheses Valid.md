---
title: leetcode-921-Minimum Add to Make Parentheses Valid
date: 2022-06-08 21:20:52
summary: 使括号有效的最少添加
categories: leetcode
tags:
 

---
## 使括号有效的最少添加
[leetcode-921-Minimum Add to Make Parentheses Valid](https://leetcode.cn/problems/minimum-add-to-make-parentheses-valid/)


```java
class Solution {
    public int minAddToMakeValid(String s) {

        char[] chars = s.toCharArray();
        int l = 0, r = 0, ans = 0;
        for (char aChar : chars) {
            if (aChar == '(') {
                l++;
            } else {
                r++;
            }
            if (r > l) {
                ans += (r - l);
                l = 0;
                r = 0;
            }
        }
        ans += -(r - l);
        return ans;
    }
}
```
