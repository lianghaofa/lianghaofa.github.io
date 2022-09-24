---
title: leetcode-135-Candy
date: 2022-05-29 19:10:52
summary: 分发糖果
categories: leetcode
tags:
- 贪心   

---
## 分发糖果
[leetcode-135-Candy](https://leetcode.cn/problems/candy/)


### 贪心

```java
public class Candy {

    public static void main(String[] args) {
        //candy(new int[]{1,3,2,2,1});
        //candy(new int[]{1,2,87,87,87,2,1});
        //candy(new int[]{5,3,7,3});
        candy(new int[]{1,2,3,5,4,3,2,1,4,3,2,1,3,2,1,1,2,3,4});
    }

    public static int candy(int[] ratings) {
        int l = 0;
        int ans = 0;
        int mL = 0;
        int mR = 0;
        int l0 = l;
        while (l < ratings.length){

            // 找递增
            l0 = l;
            while (l + 1 < ratings.length && ratings[l + 1] > ratings[l]){
                l ++;
            }

            mL = (l - l0);

            ans +=  (1 + mL) * mL / 2;
            if (l0 > 0 && ratings[l0] > ratings[l0 - 1]){
                mL ++;
                ans --;
            }
            ans +=  (1 + mL) * mL / 2;

            // 找相等
            l0 = l;
            while (l + 1 < ratings.length && ratings[l + 1] == ratings[l]){
                l ++;
            }
            int eqLen = (l - l0 + 1);

            // 找递减
            l0 = l;
            while (l + 1 < ratings.length && ratings[l + 1] < ratings[l]){
                l ++;
            }
            mR = Math.max(l - l0, 0);
            ans += (1 + mR) * mR / 2;

            // 多少个数
            if (eqLen == 1){
                ans += Math.max(mL, mR) + 1;
            }else if (eqLen == 2){
                ans += mL + 1 + mR + 1;
            }else {
                ans += mL + 1 + mR + 1 + (eqLen - 2);
            }
            l ++;
        }

        return ans;
    }
}
```
