---
title: leetcode-1358-Number of Substrings Containing All Three Characters
date: 2022-06-11 16:40:52
summary: 包含所有三种字符的子字符串数目
categories: leetcode
tags:


---
## 包含所有三种字符的子字符串数目
[leetcode-1358-Number of Substrings Containing All Three Characters](https://leetcode.cn/problems/number-of-substrings-containing-all-three-characters/)


```java
class Solution {
    public static int numberOfSubstrings(String s) {
        int[] counts = new int[3];
        char[] chars = s.toCharArray();
        int ans = 0;
        int r = 0;
        for (char aChar : chars) {
            while ((counts[0] == 0 || counts[1] == 0 || counts[2] == 0) && r < chars.length) {
                counts[chars[r++] - 'a']++;
            }
            if (counts[0] != 0 && counts[1] != 0 && counts[2] != 0) {
                ans += chars.length - r + 1;
            }
            counts[aChar - 'a']--;
        }
        return ans;
    }
}
```
