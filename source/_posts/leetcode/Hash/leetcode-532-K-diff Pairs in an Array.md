---
title: leetcode-532-K-diff Pairs in an Array
date: 2022-06-16 14:40:52
summary: 数组中的 k-diff 数对
categories: leetcode
tags:
- Hash   

---
## 数组中的 k-diff 数对
[leetcode-532-K-diff Pairs in an Array](https://leetcode.cn/problems/k-diff-pairs-in-an-array/)


```java
class Solution {
    public int findPairs(int[] nums, int k) {

        HashMap<Integer, Integer> set = new HashMap<>();
        for (int value : nums) {
            Integer v = set.get(value);
            if (v == null){
                set.put(value, 1);
            }else {
                set.put(value, 2);
            }
        }
        int ans = 0;
        if (k == 0){
            for (Map.Entry<Integer, Integer> entry : set.entrySet()){
                if (entry.getValue() > 1){
                    ans ++;
                }
            }
            return ans;
        }
        Set<Integer> visited = new HashSet<>();
        for (int num : nums) {
            if (!visited.contains(num) && set.get(num + k) != null) {
                ans++;
            }
            visited.add(num);
        }
        return ans;
    }
}
```
