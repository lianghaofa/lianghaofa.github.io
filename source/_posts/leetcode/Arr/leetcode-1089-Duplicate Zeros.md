---
title: leetcode-1089-Duplicate Zeros
date: 2022-04-25 19:40:52
summary: 复写零
categories: leetcode
tags: 

---
## 复写零
[leetcode-1089-Duplicate Zeros](https://leetcode.cn/problems/duplicate-zeros/)



```java
class Solution {
    public void duplicateZeros(int[] arr) {
        int count = 0;
        for (int value : arr) {
            if (value == 0) {
                count++;
            }
        }
        for (int i = arr.length - 1; i >= 0; i --){
            if (i + count < arr.length){
                arr[i + count] = arr[i];
            }
            if (arr[i] == 0){
                count --;
                if (i + count < arr.length ){
                    arr[i + count] = arr[i];
                }
            }
        }
    }
}
```

```java
class Solution {
    public void duplicateZeros(int[] arr) {

        int[] temp = Arrays.copyOf(arr, arr.length);
        int l = 0;
        for (int i = 0; i < arr.length; i ++, l ++){
            if (temp[l] == 0){
                arr[i] = 0;
                if (i + 1 < arr.length){
                    arr[i + 1] = 0;
                }
                i ++;
            }else {
                arr[i] = temp[l];
             }
        }
    }
}
```
