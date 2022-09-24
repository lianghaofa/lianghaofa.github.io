---
title: leetcode-2007-Find Original Array From Doubled Array
date: 2022-06-26 19:40:52
summary: 从双倍数组中还原原数组
categories: leetcode
tags:
-   

---
## 从双倍数组中还原原数组
[leetcode-2007-Find Original Array From Doubled Array](https://leetcode.cn/problems/find-original-array-from-doubled-array/)


```java
class Solution {
    public static int[] findOriginalArray(int[] changed) {

        if (changed.length % 2 == 1){
            return new int[]{};
        }
        Arrays.sort(changed);
        int[] ans = new int[changed.length / 2];
        Queue<Integer> queue = new LinkedList<>();
        int j = 0;
        for (int value : changed) {
            if (queue.isEmpty() || queue.peek() * 2 != value) {
                queue.add(value);
            } else {
                ans[j++] = queue.poll();
            }
        }
        return queue.isEmpty() ? ans : new int[]{};
    }
}
```
