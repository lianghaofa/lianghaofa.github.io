---
title: leetcode-522-Longest Uncommon Subsequence II
date: 2022-06-27 15:40:52
summary: 最长特殊序列 II
categories: leetcode
tags:


---
## 最长特殊序列 II
[leetcode-522-Longest Uncommon Subsequence II](https://leetcode.cn/problems/longest-uncommon-subsequence-ii/)


```java
class Solution {
    
    public static int findLUSlength(String[] strs) {

        // 最长的，只要相同长度的
        Arrays.sort(strs, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o2.length() - o1.length();
            }
        });
        // 最长的，只要相同长度的
        for (int i = 0; i < strs.length; i ++){
            int len = strs[i].length();
            int start = i;
            while (i + 1 < strs.length && strs[i + 1].length() == len){
                i ++;
            }
            for (int j = i; j >= start; j --){
                boolean ok = true;
                for (int k = 0; k <= i; k ++){
                    if (k != j && isSequence(strs[k].toCharArray(), strs[j].toCharArray())){
                        ok = false;
                        break;
                    }
                }
                if (ok){
                    return len;
                }
            }
        }
        return -1;
    }
    
    private static boolean isSequence(char[] longStr, char[] shortStr){
        int s = 0, l = 0;
        while (s < shortStr.length && l < longStr.length) {
            if (shortStr[s] == longStr[l]){
                s ++;
            }
            l ++;
        }
        return s == shortStr.length;
    }
}
```
