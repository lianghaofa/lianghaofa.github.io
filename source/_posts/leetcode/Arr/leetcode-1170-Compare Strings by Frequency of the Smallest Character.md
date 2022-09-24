---
title: leetcode-1170-Compare Strings by Frequency of the Smallest Character
date: 2022-06-08 19:20:52
summary: 比较字符串最小字母出现频次
categories: leetcode
tags: 

---
## 比较字符串最小字母出现频次
[leetcode-1170-Compare Strings by Frequency of the Smallest Character](https://leetcode.cn/problems/compare-strings-by-frequency-of-the-smallest-character/)



```java
class Solution {
    public static int[] numSmallerByFrequency(String[] queries, String[] words) {
        int[] sum = new int[2001];

        for (String word : words){
            sum[getFreq(word)] ++;
        }

        int s = 0;
        for (int i = 1; i < sum.length; i ++){
            s += sum[i];
            sum[i] += sum[i - 1];
        }
        
        
        int[] ans = new int[queries.length];

        for (int i = 0; i < queries.length; i ++){
            ans[i] = s - sum[getFreq(queries[i])];
        }
        return ans;
    }


    private static int getFreq(String word){

        int[] arr = new int[26];
        for (int i = 0; i < word.length(); i ++){
            arr[word.charAt(i) - 'a'] ++;
        }
        for (int i = 0; i < 26; i ++){
            if (arr[i] != 0){
                return arr[i];
            }
        }
        return 0;
    }
}
```
