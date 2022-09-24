---
title: leetcode-668-Kth Smallest Number in Multiplication Table
date: 2022-05-18 18:20:52
summary: 乘法表中第k小的数
categories: leetcode
- 二分

---
## 乘法表中第k小的数

> 

[leetcode-668-Kth Smallest Number in Multiplication Table](https://leetcode.cn/problems/kth-smallest-number-in-multiplication-table/)


![](/medias/LeetCode/1652869631.png)

![](/medias/LeetCode/1652869523.png)

![](/medias/LeetCode/1652869678.png)

![](/medias/LeetCode/1652869704.png)

![](/medias/LeetCode/1652869729.png)


```java
class Solution {
    public static int findKthNumber(int m, int n, int k) {
        int m0 = Math.max(m, n);
        int n0 = Math.min(m, n);
        int l = 1;
        int r = m0 * n0;
        int ans = -1;
        while (l <= r){
            int mid = l + (r - l >> 1);
            int count = mid / m0 * m0;
            for (int i = mid / m0 + 1; i <= n0; ++i) {
                count += mid / i;
            }
            if (count >= k){
                ans = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }
}
```

```java
    //  O( (m + n) * log(m * n)
    public static int findKthNumber0(int m, int n, int k) {
        int l = 1;
        int r = m * n;
        int ans = -1;
        while (l <= r){
            int mid = l + (r - l >> 1);
            if (findCount(m, n, mid) >= k){
                ans = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return ans;
    }

    // 小于等于 mid 的个数
    private static int findCount(int m, int n, int mid) {
        int r = m - 1;
        int c = 0;
        int count = 0;
        while (r >= 0 && c < n){
            int num = (r + 1) * (c + 1);
            while (c < n && num <= mid){
                c ++;
                num = (r + 1) * (c + 1);
            }
            count += c;
            r --;
            c --;
        }
        return count + ((r + 1) * (n + 1));
    }

```
