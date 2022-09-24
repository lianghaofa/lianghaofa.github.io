---
title: leetcode-927-Three Equal Parts
date: 2022-05-18 17:10:52
summary: 三等分
categories: leetcode
tags:
- 指针

---
## 三等分
[leetcode-927-Three Equal Parts](https://leetcode.cn/problems/three-equal-parts/)

> 把数组分成3部分
> 后缀开始遍历，后缀零
> 前缀零可忽视

### 三指针

```java
class Solution {
    public static int[] threeEqualParts(int[] arr) {
        int[] ans = new int[]{-1, -1};
        int lastOne = arr.length;
        int firstOne = -1;
        int oneCount = 0;
        for (int i = arr.length - 1; i >= 0; i --){
            if (arr[i] == 1){
                oneCount ++;
                if (lastOne == arr.length || arr[lastOne] == 0){
                    lastOne = i;
                }
                firstOne = i;
            }
        }
        if (oneCount % 3 != 0){
            return ans;
        }
        if (firstOne < 0){
            return new int[]{0, arr.length - 1};
        }
        int lastZeroCount = arr.length - lastOne - 1;
        oneCount /= 3;
        int i = firstOne;

        for (int one = 0; ;    ){
            if (arr[i] == 1){
                one ++;
            }
            if (one == oneCount){
                break;
            }
            i ++;
        }
        int  j = i + 1;
        for (int one = 0; ;    ){
            if (arr[j] == 1){
                one ++;
            }
            if (one == oneCount){
                break;
            }
            j ++;
        }
        ans[0] = i + lastZeroCount + 1;
        ans[1] = j + lastZeroCount + 1;
        i = ans[0] - 1; j = ans[1] - 1;
        int k = arr.length - 1;

        while (i >= 0 && j >= ans[0] && k >= ans[1]){
            if (arr[i] == 0 && arr[j] == 0 && arr[k] == 0 || arr[i] == 1 && arr[j] == 1 && arr[k] == 1){
                i --;
                j --;
                k --;
            }else {
                return new int[]{-1, -1};
            }
        }

        while (i >= 0){
            if (arr[i] != 0){
                return new int[]{-1, -1};
            }
            i --;
        }
        while (j >= ans[0]){
            if (arr[j] != 0){
                return new int[]{-1, -1};
            }
            j --;
        }
        while (k >= ans[1]){
            if (arr[k] != 0){
                return new int[]{-1, -1};
            }
            k --;
        }
        ans[0] -= 1;
        return ans;
    }
}
```