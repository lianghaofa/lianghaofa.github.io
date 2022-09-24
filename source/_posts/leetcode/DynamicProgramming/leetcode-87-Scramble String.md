---
title: leetcode-87-Scramble String
date: 2022-05-24 16:30:52
summary: 扰乱字符串
categories: leetcode
tags:
- DP

---
## 扰乱字符串
[leetcode-87-Scramble String](https://leetcode.cn/problems/scramble-string/)

#### 暴力递归到优化

```java
public class IsScramble {

    public static void main(String[] args) {
        System.out.println(isScramble("eebaacbcbcadaaedceaaacadccd", "eadcaacabaddaceacbceaabeccd"));
    }

    static Map<String,Boolean> sameMap;
    static Map<String,Boolean> arrMap;
    static Map<String,Boolean> checkMap;
    public static boolean isScramble(String s1, String s2) {

        int r = s1.length() - 1;

        // 左侧 与 右侧 ， 元素相等
        sameMap = new HashMap<>();
        arrMap = new HashMap<>();
        checkMap = new HashMap<>();

        return check(s1.toCharArray(), s2.toCharArray() , 0, r, 0, r);
    }


    private static boolean check(char[] s1, char[] s2 , int l1,int r1, int l2, int r2){

        String str = l1 + "-" + r1 + "-" + l2 + "-" + r2;
        Boolean b = checkMap.get(str);
        if (b != null){
            return b;
        }

        //int mid = l + (r - l >> 1);
        if (l1 == r1 && checkSame(s1, s2, l1, r1, l2, r2)){
            checkMap.put(str ,true);
            return true;
        }else {


            // 只要有一个成功即可

            for (int i = l1; i < r1; i ++ ){

                // l - i  ,  i + 1 - r
                int count = (i - l1);
                if (checkSame(s1, s2 , l1, i, l2, l2 + count) && checkSame(s1, s2 , i + 1, r1, l2 + count + 1, r2)){
                    checkMap.put(str ,true);
                    return true;
                }
                if (checkSame(s1, s2 , l1, i, l2, l2 + count) && check(s1, s2 , i + 1, r1, l2 + count + 1, r2)){
                    checkMap.put(str ,true);
                    return true;
                }
                if (checkSame(s1, s2 , i + 1, r1, l2 + count + 1, r2) && check(s1, s2 , l1, i, l2, l2 + count)  ){
                    checkMap.put(str ,true);
                    return true;
                }

                // 交换
                int rCount = (r1 - (i + 1));
                if (checkArr(s1, s2, l1, i, r2 - count, r2) && checkArr(s1, s2, i + 1, r1, l2, l2 + rCount ) &&
                        check(s1, s2, l1, i, r2 - count, r2) && check(s1, s2, i + 1, r1, l2, l2 + rCount )){
                    checkMap.put(str ,true);
                   return true;
                }

                if (checkArr(s1, s2 , l1, i, l2, l2 + count) && checkArr(s1, s2 , i + 1, r1, l2 + count + 1, r2)
                        && check(s1, s2 , l1, i, l2, l2 + count) && check(s1, s2 , i + 1, r1, l2 + count + 1, r2)){
                    checkMap.put(str ,true);
                    return true;
                }
            }
            checkMap.put(str ,false);
            return false;
        }

    }

    private static boolean checkArr(char[] s1, char[] s2 , int l1, int r1, int l2, int r2){
        String str = l1 + "-" + r1 + "-" + l2 + "-" + r2;
        Boolean b = arrMap.get(str);
        if (b != null){
            return b;
        }
        int[] arr = new int[26];
        for (int i = l1; i <= r1; i ++){
            arr[s1[i] - 'a'] ++;
        }

        for (int i = l2; i <= r2; i ++){
            arr[s2[i] - 'a'] --;
        }

        for (int value : arr) {
            if (value != 0) {
                arrMap.put(str, false);
                return false;
            }
        }
        arrMap.put(str, true);
        return true;
    }


    private static boolean checkSame(char[] s1, char[] s2 , int l1, int r1, int l2, int r2){
        String str = l1 + "-" + r1 + "-" + l2 + "-" + r2;
        Boolean b = sameMap.get(str);
        if (b != null){
            return b;
        }
        for (int i = l1, j = l2; i <= r1; i ++,j ++){
            if (s1[i] != s2[j]){
                sameMap.put(str, false);
                return false;
            }
        }
        sameMap.put(str, true);
        return true;
    }



}

```
