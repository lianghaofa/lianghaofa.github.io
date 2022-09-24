---
title: leetcode-60-Permutation Sequence
date: 2022-05-27 16:10:52
summary: 排列序列
categories: leetcode
tags:
- Math   

---
## 排列序列
[leetcode-60-Permutation Sequence](https://leetcode.cn/problems/permutation-sequence/)


```java
   public static String getPermutation(int n, int k) {
        boolean[] arr = new boolean[n];
        StringBuilder ans = new StringBuilder();
        int sum = 1;
        for (int i = 2; i < n; i ++){
            sum *= i;
        }
        k --;
        for (int i = n; i > 1; i --){
            int m = (k / sum);
            int num = getNoNum(arr, m + 1);
            ans.append(num);
            arr[num - 1] = true;
            k %= sum;
            sum /= (i - 1);
        }
        // lastNum
        for (int i = 0; i < arr.length; i ++){
            if (!arr[i]){
                ans.append(i + 1);
                break;
            }
        }
        return ans.toString();
    }


    private static int getNoNum(boolean[] arr, int no){
        int ans = arr.length;
        for (int i = 0; i < arr.length; i ++){
            if (!arr[i]){
                no --;
            }
            if (no == 0){
                return i + 1;
            }
        }
        return ans;
    }
```
