---
title: leetcode-990-Satisfiability of Equality Equations
date: 2022-05-13 12:10:52
summary: 等式方程的可满足性
categories: leetcode
tags:
- 并查集


---
## 等式方程的可满足性
[leetcode-990-Satisfiability of Equality Equations](https://leetcode.cn/problems/satisfiability-of-equality-equations/)


### 并查集 


```java
public class EquationsPossible {

    public boolean equationsPossible(String[] equations) {
        int[] arr = new int[26];
        for (int i = 0; i < arr.length; i ++){
            arr[i] = i;
        }
        for (String s : equations) {
            if (s.charAt(1) == '=') {
                arr[findParent(arr, s.charAt(0) - 'a')] = findParent(arr, s.charAt(3) - 'a');
            }
        }
        for (String equation : equations) {
            if (equation.charAt(1) == '!' &&
                    findParent(arr, equation.charAt(0) - 'a') == findParent(arr, equation.charAt(3) - 'a')) {
                return false;
            }
        }
        return true;
    }

    private int findParent(int[] arr, int cur){
        if (arr[cur] != cur){
            arr[cur] = findParent(arr, arr[cur]);
        }
        return arr[cur];
    }
}
```
