---
title: leetcode-One Away LCCI
date: 2022-05-09 09:40:52
summary: 一次编辑
categories: leetcode
tags:
- 双指针

---
## 一次编辑
[leetcode-One Away LCCI](https://leetcode.cn/problems/one-away-lcci/)

```java
class Solution {
    public boolean oneEditAway(String first, String second) {

        int diff = Math.abs(first.length() - second.length());

        if (diff > 1){
            return false;
        }else if (diff == 0){
            int count = 0;
            for (int i = 0; i < first.length(); i ++){
                if (first.charAt(i) != second.charAt(i)){
                    count ++;
                }
            }
            return count <= 1;
        }else {
            // diff  == 1
            char[] longOne = first.length() > second.length() ? first.toCharArray() : second.toCharArray();
            char[] shortOne = first.length() > second.length() ? second.toCharArray() : first.toCharArray();
            return oneChange(longOne, shortOne);

        }
    }


    private boolean oneChange(char[] longOne, char[] shortOne){
        for (int i = 0; i < longOne.length && i < shortOne.length; i++){
            if (longOne[i] != shortOne[i]){
                return isSame(longOne, i + 1 ,shortOne, i);
            }
        }
        return true;
    }

    private boolean isSame(char[] longOne, int s0 , char[] shortOne, int s1){
        if (longOne.length - s0 != shortOne.length - s1){
            return false;
        }
        for (int i = s0, j = s1; i < longOne.length; i ++, j ++){
            if (longOne[i] != shortOne[j]){
                return false;
            }
        }
        return true;
    }
}
```

#### 双指针
```java
class Solution {
    public boolean oneEditAway(String first, String second) {
        char[] f = first.toCharArray();
        char[] s = second.toCharArray();
        int fl = 0;
        int fr = first.length() - 1;
        int sl = 0;
        int sr = second.length() - 1;
        while (fl < first.length() && sl < second.length() && f[fl] == s[sl]){
            fl ++;
            sl ++;
        }
        while (fr >= 0 && sr >= 0 && f[fr] == s[sr]){
            fr --;
            sr --;
        }
        return Math.max(fr - fl , sr - sl) < 1;
    }
}
```
