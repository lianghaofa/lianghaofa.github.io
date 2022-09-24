---
title: leetcode-2001-Number of Pairs of Interchangeable Rectangles
date: 2022-06-03 20:20:52
summary: 可互换矩形的组数
categories: leetcode
tags:
 

---
## 可互换矩形的组数
[leetcode-2001-Number of Pairs of Interchangeable Rectangles](https://leetcode.cn/problems/number-of-pairs-of-interchangeable-rectangles/)



```java
class Solution {
    public long interchangeableRectangles(int[][] rectangles) {
        long ans = 0;
        Map<Double, Integer> map = new HashMap<>();
        for (int i = rectangles.length - 1; i >= 0; i --){
            double r = rectangles[i][0] / (double)(rectangles[i][1]);
            Integer v = map.get(r);
            if (v == null){
                map.put(r, 1);
            }else {
                ans += v;
                map.put(r, v + 1);
            }
        }
        return ans;
    }
}
```
