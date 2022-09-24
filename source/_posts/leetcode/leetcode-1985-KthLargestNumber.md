---
title: leetcode-1985-KthLargestNumber
date: 2022-05-07 14:45:52
summary: 找出数组中的第 K 大整数
categories: leetcode
tags:


---
## 找出数组中的第 K 大整数
[leetcode-1985-KthLargestNumber](https://leetcode-cn.com/problems/find-the-kth-largest-integer-in-the-array/)



```java
class Solution {
    public String kthLargestNumber(String[] nums, int k) {
        Arrays.sort(nums, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                if (o1.length() != o2.length()){
                    return o2.length() - o1.length();
                }
                return o2.compareTo(o1);
            }
        });
        return nums[k - 1];
    }
}
```
