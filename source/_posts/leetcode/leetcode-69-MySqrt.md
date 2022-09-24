---
title: leetcode-69-MySqrt
date: 2022-04-25 14:40:52
summary: x 的平方根
categories: leetcode
tags:
- 二分

---
## x 的平方根
[leetcode-69-MySqrt](https://leetcode-cn.com/problems/sqrtx/)


### 二分

```java
class Solution {
    public int mySqrt(int x) {
        int ans = 0;
        int l = 0;
        int r = x;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if ((long)mid * mid == x){
                return mid;
            }else if ((long)mid * mid > x){
                r = mid - 1;
            }else {
                ans = mid;
                l = mid + 1;
            }
        }
        return ans;
    }
}
```

### 牛顿迭代