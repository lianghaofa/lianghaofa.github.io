---
title: leetcode-215-FindKthLargest
date: 2022-04-24 12:40:52
summary: 分割回文串 II
categories: leetcode
tags:
- 回文串


---
## FindKthLargest
[leetcode-215-MinCut](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)


### 随机快排

```java
public class MinCut {

    public static int findKthLargest(int[] piles, int k){

        int index = piles.length - k;
        int start = 0;
        int end = piles.length - 1;
        int[] res=  quickSortLike(piles, start, end);
        while (!(index >= res[0] && index <= res[1])){
            if (index < res[0]){
                end = res[0] - 1;
                res =  quickSortLike(piles, start, end);
            }else {
                start = res[1] + 1;
                res =  quickSortLike(piles, start, end);
            }
        }
        return piles[index];

    }

    private static int[] quickSortLike(int[] piles, int start, int end){

        int target = piles[start + (int)(Math.random() * (end - start + 1))];
        int l = start - 1;
        int r = end + 1;
        int left = start;
        while (left < r){
            if (piles[left] > target){
                swap(piles,left, -- r);
            }else if (piles[left] == target){
                left ++;
            }else {
                swap(piles,++ l, left ++);
            }
        }
        return new int[]{l + 1,r - 1};

    }

    private static void swap(int[] piles, int i, int i1) {
        int temp = piles[i];
        piles[i] = piles[i1];
        piles[i1] = temp;
    }
}
```