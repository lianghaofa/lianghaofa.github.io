---
title: leetcode-312-Burst Balloons
date: 2022-06-18 18:20:52
summary: 戳气球
categories: leetcode
tags:
- DP

---
## 戳气球
[leetcode-312-Burst Balloons](https://leetcode.cn/problems/burst-balloons/)

#### 暴力递归

```java
public class MaxCoins {

    public static int maxCoins(int[] nums) {

        // 最大值
        int[] arr = new int[nums.length + 2];
        arr[0] = 1;
        arr[arr.length - 1] = 1;
        for (int i = 1; i < arr.length - 1; i ++){
            arr[i] = nums[i - 1];
        }
        Map<String,Integer> map = new HashMap<>();
        return getMax(arr, 0,  arr.length - 1, map);
    }


    private static int getMax(int[] arr,int l, int r, Map<String,Integer> map){


        if (l + 1 >= r){
            return 0;
        }
        String s = l + "-" + r;
        Integer val = map.get(s);
        if (val != null){
            return val;
        }
        int mid = l + 1;
        int max = Integer.MIN_VALUE;
       while (mid < r){
          int res = getMax(arr, l,  mid, map) + getMax(arr, mid,  r, map) + arr[l] * arr[mid] * arr[r];
          max = Math.max(res, max);
          mid ++;
       }
        map.put(s, max);
       return max;
    }

}

```

#### 优化


```java
public class MaxCoins {

public static void main(String[] args) {
        System.out.println(maxCoins(new int[]{21,47,65,65,26,17,80,25,92,42,1,5,20,75,98,42,61,76,2,95,76,12,87,87,71,71,38,95,49,61,85,50,8,83,36,16,12,49,51,85,7,29,26,17,61,57,34,89,89,16,9,79,11,75,65,61,78,45,67,14,59,21,67,82,27,14,36,94,65,52,38,89,29,75,52,28,87,22,4,12,20,41,63,78,30,92,26,40,0,52,65,51,30,34,71,64,74,48,49,80,45,3,65,27,90,23,88,56,64,97,88,97,70,25,93,41,63,0,18,74,41,41,63,72,29,97,24,46,3,50,32,16,95,80,98,14,27,25,81,34,27,62,84,2,93,72,34,61,92,94,59,88,75,12,3,10,60,22,71,47,49,1,39,48,2,99,53,32,32,18,51,4,43,77,78,62,42,76,82,70,57,67,37,87,75,72,0,42,43,73,67,91,22,14,69,38,100,40,100,15,26,91,1,67,28,58,92,16,11,78,7,53,28,65,48,90,78,92,49,78,38,3,79,13,59,41,7,8,27,18,71,97,71,14,61,58,86,84,0,8,97,51,65,69,39,24,20,62,45,97,42,12,92,52,7,46,8,56,20,22,11,12,81,27,12,15,19,1,84,14,38,29,69,42,44,66,68,22,24,94,31,62,36,37,75,6,7,26,69,17,67,52,12,100,88,10,16,40,93,62}));
    }

    public static int maxCoins(int[] nums) {

        // 最大值
        int[] arr = new int[nums.length + 2];
        arr[0] = 1;
        arr[arr.length - 1] = 1;
        System.arraycopy(nums, 0, arr, 1, arr.length - 1 - 1);
        int[][] dp = new int[arr.length][arr.length];
        for (int l = arr.length - 3; l >= 0; l --){
            for (int r = l + 2; r < arr.length; r ++){
                for (int mid = l + 1; mid < r; mid ++){
                    dp[l][r] = Math.max(dp[l][r], arr[l] * arr[mid] * arr[r] + dp[l][mid] + dp[mid][r]);
                }
            }
        }
        return dp[0][dp[0].length - 1];
    }
    

}

```




