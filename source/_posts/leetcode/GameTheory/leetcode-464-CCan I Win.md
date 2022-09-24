---
title: leetcode-464-CCan I Win
date: 2022-05-22 21:40:52
summary: 我能赢吗
categories: leetcode
tags:
- Game Theory   

---
## 组合
[我能赢吗](https://leetcode.cn/problems/can-i-win/)

> 暴力尝试
> 记忆化搜索 - 存储已经尝试过的记录，再次遍历直接获取


### 最直观的暴力解

```java
class Solution {
    
    public static boolean canIWin(int maxChoosableInteger, int desiredTotal) {

        boolean[] dp = new boolean[maxChoosableInteger + 1];

        return dfs0(dp, desiredTotal, 0);
    }

    private static boolean dfs0(boolean[] dp, int desiredTotal, int sum){

        for (int i =  dp.length - 1; i >= 1; i --){
            if (!dp[i]){
                dp[i] = true;

                if (sum + i >= desiredTotal) {
                    dp[i] = false;
                    return true;
                }

                // 当前玩家选择了 x 以后，判断对方玩家一定输吗？
                if (dfs1(dp, desiredTotal, sum + i)) {
                    dp[i] = false;
                    return true;
                }
                dp[i] = false;
            }
        }
        return false;

    }

    private static boolean dfs1(boolean[] dp, int desiredTotal, int sum){

        for (int i =  dp.length - 1; i >= 1; i --){
            if (!dp[i]){
                dp[i] = true;
                if (sum + i >= desiredTotal) {
                    dp[i] = false;
                    return false;
                }
                // 当前玩家选择了 x 以后，判断对方玩家一定输吗？
                if (!dfs0(dp, desiredTotal, sum + i)) {
                    dp[i] = false;
                    return false;
                }
                dp[i] = false;
            }
        }
        return true;
    }
}
```

### 暴力解的优化

```java
class Solution {
    
    public static boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        if (desiredTotal > (1 + maxChoosableInteger) * maxChoosableInteger / 2){
            return false;
        }
        boolean[] dp = new boolean[maxChoosableInteger + 1];

        return dfs(dp, desiredTotal, 0);
    }

    private static boolean dfs(boolean[] dp, int desiredTotal, int sum){
        
        for (int i =  dp.length - 1; i >= 1; i --){
            if (!dp[i]){
                dp[i] = true;
                if (sum + i >= desiredTotal) {
                    dp[i] = false;
                    return true;
                }
                
                if (!dfs(dp, desiredTotal, sum + i)) {
                    dp[i] = false;
                    return true;
                }
                dp[i] = false;
            }
        }
        return false;
    }
}
```

### 记忆化优化

```java
class Solution {
    
    Map<Integer,Boolean> map;
    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        if (desiredTotal > (1 + maxChoosableInteger) * maxChoosableInteger / 2){
            return false;
        }
        boolean[] dp = new boolean[maxChoosableInteger + 1];
        map = new HashMap<>();
        return dfs(dp, desiredTotal, 0, 0);
    }

    private boolean dfs(boolean[] dp, int desiredTotal, int sum,int cur){
        Boolean b = map.get(cur);
        if (b != null){
            return b;
        }
        for (int i =  dp.length - 1; i >= 1; i --){
            if (!dp[i]){
                if (sum + i >= desiredTotal) {
                    map.put(setBits(cur, i), true);
                    return true;
                }
                dp[i] = true;
                if (!dfs(dp, desiredTotal, sum + i, setBits(cur, i))) {
                    dp[i] = false;
                    map.put(setBits(cur, i), false);
                    map.put(cur, true);
                    return true;
                }
                dp[i] = false;
            }
        }
        map.put(cur, false);
        return false;
    }

    private int setBits(int cur, int index){
        return cur | (1 << (index - 1));
    }
}
```

