---
title: leetcode-1573-Number of Ways to Split a String
date: 2022-07-06 15:40:52
summary: 分割字符串的方案数
categories: leetcode
tags:


---
## 分割字符串的方案数
[leetcode-1573-Number of Ways to Split a String](https://leetcode.cn/problems/number-of-ways-to-split-a-string/)


```java
class Solution {
    public static int numWays(String s) {

        int oneNums = 0;
        char[] chars = s.toCharArray();
        for (char c : chars){
            if (c == '1'){
                oneNums ++;
            }
        }
        int len = chars.length;
        if (oneNums % 3 != 0 || len < 3){
            return 0;
        }
        if (oneNums == 0){
            int c = (len - 2) - 1 + 1;
            return (int) (((1L + (len - 2L)) * c / 2L)  % 1000000007);
        }
        oneNums /= 3;
        int l0 = 0;
        int one = 0;
        for (; l0 < len; l0 ++){
            if (chars[l0] == '1'){
                one ++;
                if (one == oneNums){
                    break;
                }
            }
        }
        int l1 = l0 + 1;
        while (l1 < len && chars[l1] == '0'){
            l1 ++;
        }
        long l = l1 - l0;
        int r0 = len - 1;
        one = 0;
        for (; r0 >= 0; r0 --){
            if (chars[r0] == '1'){
                one ++;
                if (one == oneNums){
                    break;
                }
            }
        }
        int r1 = r0 - 1;
        while (r1 >= 0 && chars[r1] == '0'){
            r1 --;
        }
        long r = r0 - r1;
        return (int) ((l * r) % 1000000007);
    }
}
```
