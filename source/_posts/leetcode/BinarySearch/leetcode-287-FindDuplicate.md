---
title: leetcode-287-FindDuplicate
date: 2022-05-05 19:50:52
summary: 寻找重复数
categories: leetcode
- 二分

---
## 寻找重复数


[leetcode-33-Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

### 二分 O(nlogn),O(1)

1. 确定区间 [1, n]
2. 不断取值，暴力查找是否符合。

二分太暴力。

```java
    public int findDuplicate(int[] nums) {
        int n = nums.length;
        int l = 1, r = n - 1, ans = -1;
        while (l <= r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int i = 0; i < n; ++i) {
                if (nums[i] <= mid) {
                    cnt++;
                }
            }
            if (cnt <= mid) {
                l = mid + 1;
            } else {
                r = mid - 1;
                ans = mid;
            }
        }
        return ans;
    }
```


### 原地加数 O(n), O(1)

> nums[i] 对应 nums[i] - 1 位置
> 区间 [1,nums.length]


m 不建议等于 nums.length，边界不好处理。
只要比nums.length大，且不溢出都好处理。

```java
    public int findDuplicate(int[] nums) {

        // nums[i] 对应 nums[i] - 1 位置

        // 区间 [1,nums.length]
        int ans = 0;
        int m = nums.length + 1;

        for (int i = 0; i < nums.length; i ++){
            nums[(nums[i] - 1) % m] += m;
        }

        // 恢复
        for (int i = 0; i < nums.length ; i ++){
            int m2 = 2 * m;
            if (nums[i] >= m2){
                ans = i + 1;
            }
            if (nums[i] >= m2){
                nums[i] -= m2;
            }else if (nums[i] >=  m){
                nums[i] -= m;
            }
            System.out.println(nums[i]);
        }
        return ans;
    }
```


###  快慢指针  O(n), O(1)

检测链表的思路， 边界不好code。

面试建议 方法二

```java
    public int findDuplicate(int[] nums) {
        int n = nums.length;
        int l = 1, r = n - 1, ans = -1;
        while (l <= r) {
            int mid = (l + r) >> 1;
            int cnt = 0;
            for (int i = 0; i < n; ++i) {
                if (nums[i] <= mid) {
                    cnt++;
                }
            }
            if (cnt <= mid) {
                l = mid + 1;
            } else {
                r = mid - 1;
                ans = mid;
            }
        }
        return ans;
    }
```