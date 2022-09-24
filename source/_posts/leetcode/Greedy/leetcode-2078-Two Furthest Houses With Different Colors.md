---
title: leetcode-2078-Two Furthest Houses With Different Colors
date: 2022-06-21 19:10:52
summary: 两栋颜色不同且距离最远的房子
categories: leetcode
tags:
- 贪心   

---
## 两栋颜色不同且距离最远的房子
[leetcode-2078-Two Furthest Houses With Different Colors](https://leetcode.cn/problems/two-furthest-houses-with-different-colors/)


### 贪心

```java
public class MaxDistance {
    public int maxDistance(int[] colors) {
        int ans = 0;
        for (int i = 0; i < colors.length; i ++){
            if (colors[i] != colors[colors.length - 1]){
                ans = colors.length - 1 - i;
                break;
            }
        }
        for (int i = colors.length - 1; i >= 0; i --){
            if (colors[i] != colors[0]){
                ans = Math.max(ans, i);
                break;
            }
        }
        return ans;
    }
}
```
