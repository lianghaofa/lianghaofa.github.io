---
title: leetcode-791-Custom Sort String
date: 2022-06-23 19:40:52
summary: 自定义字符串排序
categories: leetcode
tags:
-   

---
## 自定义字符串排序
[leetcode-791-Custom Sort String](https://leetcode.cn/problems/custom-sort-string/)


```java
class Solution {
    public String customSortString(String order, String s) {

        int[] count = new int[26];
        for (int i = 0; i < s.length(); i ++){
            count[s.charAt(i) - 'a'] ++;
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < order.length(); i ++){
            for(int j = 0; j < count[order.charAt(i) - 'a']; j ++){
                ans.append(order.charAt(i));
            }
            count[order.charAt(i) - 'a'] = 0;
        }
        for (int i = 0; i < 26; i ++){
            for(int j = 0; j < count[i]; j ++){
                ans.append((char) (i + 'a'));
            }
        }
        return ans.toString();
    }
}
```
