---
title: leetcode-1574-Shortest Subarray to be Removed to Make Array Sorted
date: 2022-05-15 09:10:52
summary: 删除最短的子数组使剩余数组有序
categories: leetcode
tags:
- 双指针

---
## 删除最短的子数组使剩余数组有序
[leetcode-1574-Shortest Subarray to be Removed to Make Array Sorted](https://leetcode.cn/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)


### 双指针

```java
class Solution {
    public int findLengthOfShortestSubarray(int[] arr) {
        int left = 0;
        while(left + 1 < arr.length && arr[left] <= arr[left+1]) {
            left++;
        }
        if(left == arr.length - 1) {
            return 0;
        }

        int right = arr.length - 1;
        while(right > left && arr[right-1] <= arr[right]) {
            right--;
        }
        int ans = Math.max(arr.length - right, left + 1);
        int i = 0;
        int j = right;
        while (i <= left && j < arr.length){
            if (arr[j] >= arr[i]){
                ans = Math.max(ans, arr.length - j + i + 1);
                i ++;
            }else {
                j ++;
            }

        }
        return arr.length - ans;
    }
}
```