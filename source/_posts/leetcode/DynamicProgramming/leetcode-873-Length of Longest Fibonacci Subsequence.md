---
title: leetcode-873-Length of Longest Fibonacci Subsequence
date: 2022-05-10 09:40:52
summary: 最长的斐波那契子序列的长度
categories: leetcode
tags:
- DP

---
## 最长的斐波那契子序列的长度
[leetcode-873-Length of Longest Fibonacci Subsequence](https://leetcode.cn/problems/length-of-longest-fibonacci-subsequence/)


#### 暴力

```java
    public int lenLongestFibSubseq(int[] A) {
        int N = A.length;
        Map<Integer, Integer> index = new HashMap();
        for (int i = 0; i < N; ++i) {
            index.put(A[i], i);
        }

        Map<Integer, Integer> longest = new HashMap();
        int ans = 0;
        for (int k = 0; k < N; ++k) {
            for (int j = k - 1; j >= 0; j --) {
                int i2 = A[k];
                int i1 = A[j];
                int i0 = i2 - i1;
                int last = j;
                int i = index.getOrDefault(i0, -1);
                int len = 2;
                while (i >= 0 && i < last){
                    i2 = i1;
                    i1 = i0;
                    i0 = i2 - i1;
                    last = i;
                    i = index.getOrDefault(i0, -1);
                    
                    len ++;
                }
                ans = Math.max(ans, len);
            }
        }
        return ans >= 3 ? ans : 0;
    }
```

#### 优化
```java
    public static int lenLongestFibSubseq(int[] A) {
        int N = A.length;
        Map<Integer, Integer> index = new HashMap();
        for (int i = 0; i < N; ++i) {
            index.put(A[i], i);
        }

        Map<Integer, Integer> longest = new HashMap();
        int ans = 0;
        for (int k = 0; k < N; k ++) {
            for (int j = k - 1; j >= 0; j --) {
                int i2 = A[k];
                int i1 = A[j];
                // 建立 longest 的映射关系
                //  (k,j) -> j * N + k;
                int map = j * N + k;
                int i0 = i2 - i1;
                int last = j;
                int len = 2;
                int i = index.getOrDefault(i0, -1);
                if (i >= 0 && i < j){
                    i = longest.getOrDefault(map, -1);
                    if (i >= 0){
                        len = i + 1;
                    }else {
                        len = 2;
                        //  (i,j) -> i * N + j;
                        map = i * N + j;
                        i = index.getOrDefault(i0, -1);
                        while (i >= 0 && i < last){
                            i2 = i1;
                            i1 = i0;
                            i0 = i2 - i1;
                            last = i;
                            i = index.getOrDefault(i0, -1);
                            len ++;
                        }
                        longest.put(map,len);
                    }
                }
                ans = Math.max(ans, len);
            }
        }
        return ans >= 3 ? ans : 0;
    }

```