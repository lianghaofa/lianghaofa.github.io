---
title: leetcode-1870-Minimum Speed to Arrive on Time.
date: 2022-05-17 13:50:52
summary: 准时到达的列车最小时速
categories: leetcode
- 二分

---
## 准时到达的列车最小时速


[leetcode-1870-Minimum Speed to Arrive on Time.](https://leetcode.cn/problems/minimum-speed-to-arrive-on-time/)


```java
class Solution {
    public int minSpeedOnTime(int[] dist, double hour) {
        int  l = 1;
        int r = 10_000_000;
        int ans = -1;
        while (l <= r){
            int speed = l + (r - l >> 1);
            if (canWork(dist, speed, hour)){
                ans = speed;
                r = speed - 1;
            }else {
                l = speed + 1;
            }
        }
        return ans;
    }

    private boolean canWork(int[] dist, int speed, double hour) {
        double c = 0;
        for (int i = 0; i < dist.length - 1;i ++){
            c += (dist[i] + speed - 1) / speed;;
            if (c > hour){
                return false;
            }
        }
        c += dist[dist.length - 1] / (double) (speed);
        return c <= hour;
    }
}
```
