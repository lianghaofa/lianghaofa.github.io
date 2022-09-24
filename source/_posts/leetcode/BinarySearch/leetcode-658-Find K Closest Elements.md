---
title: leetcode-658-Find K Closest Elements
date: 2022-05-16 18:30:52
summary: 找到 K 个最接近的元素
categories: leetcode
- 二分

---
## 找到 K 个最接近的元素

> 有序
> 二分查询到最接近的数
> 双指针外两边扩

[leetcode-658-Find K Closest Elements](https://leetcode.cn/problems/find-k-closest-elements/)

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        List<Integer> list = new ArrayList<>();
        int l = 0;
        int r = arr.length - 1;
        int index = arr.length;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (arr[mid] >= x){
                index = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        r = index;
        if (index == arr.length){
            // index - 1
            l = index - 2;
        }else if (index == 0 || arr[index] == x || Math.abs(arr[index] - x) < Math.abs(arr[index - 1] - x) ){
            // index
            l = index - 1;
            r = index + 1;
        }else {
            // index - 1
            l = index - 2;
        }
        while (k > 1 && (l >= 0 || r < arr.length)){
            if (l < 0){
                r ++;
            }else if (r >= arr.length){
                l --;
            }else if (Math.abs(arr[l] - x) > Math.abs(arr[r] - x)){
                r ++;
            }else {
                l --;
            }
            k --;
        }
        for (int i = l + 1 ; i < r; i ++){
            list.add(arr[i]);
        }
        return list;
    }
}
```
