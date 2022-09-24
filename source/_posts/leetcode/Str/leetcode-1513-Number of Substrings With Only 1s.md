---
title: leetcode-1513-Number of Substrings With Only 1s
date: 2022-06-23 21:30:52
summary: 仅含 1 的子串数
categories: leetcode
tags:


---
## 仅含 1 的子串数
[leetcode-1513-Number of Substrings With Only 1s](https://leetcode.cn/problems/number-of-substrings-with-only-1s/)


```java
class Solution {
    public int numSub(String s) {

        char[] chars = s.toCharArray();
        int r = 0;
        long ans = 0;
        for (int i = 0; i < chars.length && r < chars.length; ){
            if (chars[i] == '1'){
                r = i + 1;
                while (r < chars.length && chars[r] == '1'){
                    r ++;
                }
                long len = r - i;
                ans += (1 + len) * len / 2;
                ans %= 1000000007;
                i = r + 1;
            }else {
                i ++;
            }
        }
        return (int) ans;
    }
}
```
