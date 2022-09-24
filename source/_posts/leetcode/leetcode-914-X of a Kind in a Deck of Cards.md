---
title: leetcode-914-leetcode-914-X of a Kind in a Deck of Cards
date: 2022-05-06 14:40:52
summary: 卡牌分组
categories: leetcode


---
## 卡牌分组
[leetcode-914-leetcode-914-X of a Kind in a Deck of Cards](https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards/)

```java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        int[] count = new int[10000];
        for (int item : deck) {
            count[item]++;
        }
        int g0 = -1;
        for (int value : count) {
            if (value == 0) {
                continue;
            }
            if (g0 == -1) {
                g0 = value;
            } else {
                g0 = gcd(g0, value);
            }
        }
        return g0 >= 2;
    }

    private int gcd(int a, int b){
        if (b == 0){
            return a;
        }
        return gcd(b, a % b);
    }
}
```

### 牛顿迭代