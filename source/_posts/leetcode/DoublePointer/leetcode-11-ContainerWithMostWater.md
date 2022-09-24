---
title: leetcode-11-Container With Most Water
date: 2022-05-06 09:10:52
summary: 盛最多水的容器
categories: leetcode
tags:
- 双指针

---
## 盛最多水的容器
[leetcode-11-Container With Most Water](https://leetcode-cn.com/problems/container-with-most-water/)

> 区间 [a,b] 能够存储的水量，由 height[a], height[b] 的最小值决定。
> 小的不断向令一侧移动

### 双指针

```java
class Solution {
    public int maxArea(int[] height) {
        int l = 0;
        int r = height.length - 1;
        int ans = Integer.MIN_VALUE;
        while (l < r){
            int h = Math.min(height[l], height[r]);
            ans =  Math.max(ans, (r - l) * h);
            if (h == height[l]){
                l ++;
            }else {
                r --;
            }
        }
        return ans;
    }
}
```