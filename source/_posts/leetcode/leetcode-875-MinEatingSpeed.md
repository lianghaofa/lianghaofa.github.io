---
title: leetcode-875-MinEatingSpeed
date: 2022-04-24 14:40:52
summary: 爱吃香蕉的珂珂
categories: leetcode
tags:
- 二分查找
---
## MinEatingSpeed
[leetcode-875-MinEatingSpeed](https://leetcode-cn.com/problems/koko-eating-bananas/)

>从最慢开始，让珂珂一小时只吃 1 个，看珂珂能否在 h 小时内吃完。
让珂珂一小时只吃 2 个，看珂珂能否在 h 小时内吃完。
如果一小时吃 k 个 吃完，返回 k。
min = 1;
max = Math.max(max, pile);

{% video <iframe width="560" height="315" src="https://www.youtube.com/embed/YES16mVB0lQ?rel=0&amp;showinfo=0" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe> %}

### 爱吃香蕉的珂珂

```java
public class MinEatingSpeed {

    public static void main(String[] args) {
        minEatingSpeed(new int[]{332484035,524908576,855865114,632922376,222257295,690155293,112677673,679580077,337406589,290818316,877337160,901728858,679284947,688210097,692137887,718203285,629455728,941802184}, 823855818);


    }

    public static int minEatingSpeed(int[] piles, int h) {
        if (h < piles.length){
            return -1;
        }
        int max = Integer.MIN_VALUE;
        for(int pile : piles){
            max = Math.max(max, pile);
        }
        int l = 1, r = max, ans = max;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (canFinish( piles, mid, h)){
                ans = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }

    private static boolean canFinish(int[] piles, int speed, int h){
        for (int pile : piles) {
            h -= (pile + speed - 1) / speed;
            if (h < 0) {
                return false;
            }
        }
        return true;
    }
}
```