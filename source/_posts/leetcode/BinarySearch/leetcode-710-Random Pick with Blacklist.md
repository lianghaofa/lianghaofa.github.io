---
title: leetcode-710-Random Pick with Blacklist
date: 2022-06-26 17:20:52
summary: 黑名单中的随机数
categories: leetcode
- 二分

---
## 黑名单中的随机数

[leetcode-710-Random Pick with Blacklist](https://leetcode.cn/problems/random-pick-with-blacklist/)


```java
class Solution {

    int[] blackList;
    int n;

    public Solution(int n, int[] blacklist) {
        this.n = n;
        this.blackList = blacklist;
        Arrays.sort(this.blackList);
    }

    public int pick() {
        int last = n - blackList.length;
        int ans = blackList.length;
        int random = (int) (Math.random() * last) + 1;
        // 二分
        int l = 0, r = blackList.length - 1;
        while (l <= r){
            int mid = l + (r - l >> 1);
            // blackList[mid] - mid  左侧有多少个数
            int leftCount  = (blackList[mid]) - (mid);
            if (leftCount >= random){
                ans = mid;
                r = mid  - 1;
            }else {
                l = mid  + 1;
            }
        }
        return (random - 1) +  ans;
    }
}
```
