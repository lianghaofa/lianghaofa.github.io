---
title: leetcode-2086-Minimum Number of Buckets Required to Collect Rainwater from Houses
date: 2022-06-22 17:10:52
summary: 从房屋收集雨水需要的最少水桶数
categories: leetcode
tags:
- 贪心   

---
## 分发糖果
[leetcode-2086-Minimum Number of Buckets Required to Collect Rainwater from Houses](https://leetcode.cn/problems/minimum-number-of-buckets-required-to-collect-rainwater-from-houses/)


### 贪心

```java
class Solution {
    public static int minimumBuckets(String street) {

        char[] chars = street.toCharArray();
        int ans = 0;
        for (int i = 0; i < chars.length; i ++){
            if (chars[i] == 'H'){
                if (i + 1 < chars.length && chars[i + 1] == '.'){
                    if (i + 2 < chars.length && chars[i + 2] == 'H'){
                        chars[i + 2] = 'X';
                        i ++;
                    }
                    i ++;
                    ans ++;
                } else if (i - 1 >= 0 && chars[i - 1] == '.'){
                    ans ++;
                }else {
                    return -1;
                }
            }
        }
        return ans;
    }
}
```
