---
title: leetcode-1103-DistributeCandies
date: 2022-05-20 15:10:52
summary: 分糖果 II
categories: leetcode
tags:
- Math   

---
## 分糖果 II
[leetcode-1103-DistributeCandies](https://leetcode.cn/problems/distribute-candies-to-people/)


```java
class Solution {
    public static  int[] distributeCandies(int candies, int num_people) {

        int[] ans = new int[num_people];
        int first = 1, firstSum = 0, end = first + num_people - 1, sum = 0, d = 0;
        while ((sum + (first + end) / 2 * num_people) < candies){
            sum += (first + end) * num_people / 2;
            firstSum += first;
            first = end + 1;
            d ++;
            end = first + num_people - 1;
        }
        // firstSum 上一层次的第一个数
        for (int i = 0; i < ans.length; i ++){
            if (sum >= candies){
                ans[i] = firstSum;
            }else {
                if (sum + first >= candies){
                    ans[i] = firstSum + (candies - sum);
                }else {
                    ans[i] = firstSum + first;
                }
                sum += first;
            }
            firstSum += d;
            first ++;
        }
        return ans;
    }
}
```
