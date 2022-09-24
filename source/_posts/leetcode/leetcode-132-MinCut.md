---
title: leetcode-132-MinCut
date: 2022-04-23 12:40:52
summary: 分割回文串 II
categories: leetcode
tags:
- 回文串
- 回文串

---
## MinCut
[leetcode-132-MinCut](https://leetcode-cn.com/problems/palindrome-partitioning-ii/)



```java
public class MinCut {

    public static void main(String[] args) {
        System.out.println(minCut("aabbbc"));
    }

    static int[] res;
    public static int minCut(String s) {
        res = manacher(s);
        if (isPalindrome(0,s.length() - 1)){
            return 0;
        }
        // 包含 i 的最少分割次数
        int[] f = new int[s.length()];
        for (int i = 1; i < s.length(); i ++){
            int count = Integer.MAX_VALUE;
            if (isPalindrome(0,i)){
                f[i] = 0;
            }else {
                for (int j = 1; j <= i; j ++){
                    if (isPalindrome(j,i)){
                        count = Math.min(f[j - 1] + 1,count);
                    }
                }
                f[i] = count;
            }
        }
        return f[s.length() - 1];
    }

    private static int[] manacher(String s){
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append("#");
        for (int i = 0; i < s.length(); i ++){
            stringBuilder.append(s.charAt(i));
            stringBuilder.append('#');
        }
        // int l = -1;
        int right = 0,center = 0;
        int[] res = new int[stringBuilder.length()];
        for (int i = 1; i < stringBuilder.length() - 1; i ++){
            res[i] = i <= right ? Math.min(right - i + 1, res[2 * center - i]) : 1;
            while (i + res[i] < stringBuilder.length() && i - res[i] >= 0 && stringBuilder.charAt(i + res[i]) == stringBuilder.charAt(i - res[i])){
                res[i] ++;
            }
            if (i + res[i]  - 1 > right){
                right = i + res[i] - 1;
                center = i;
            }
            res[i] --;
        }
        return  res ;
    }


    private static boolean isPalindrome(int start,int end){
        if (start > end){
            return false;
        }
        if (start == end){
            return true;
        }
        int mapI = (start) * 2 + 1;
        int mapJ = (end) * 2 + 1;
        int mapC = mapI + ((mapJ - mapI) >> 1);
        int mapCenterRadius = res[mapC];
        // 判断 start - i 是否回文
        return mapC + mapCenterRadius >= mapJ;
    }
}
```