---
title: leetcode-508-Most Frequent Subtree Sum
date: 2022-06-19 14:40:52
summary: 出现次数最多的子树元素和
categories: leetcode
tags:


---
## 出现次数最多的子树元素和
[leetcode-508-Most Frequent Subtree Sum](https://leetcode.cn/problems/most-frequent-subtree-sum/)


```java
class Solution {
    Map<Integer, Integer> freqMap;
      int feq = 0;
    public int[] findFrequentTreeSum(TreeNode root) {
        freqMap = new HashMap<>();
        feq = 0;
        transverse(root);
        List<Integer> list = new ArrayList<>();
        for (Map.Entry<Integer, Integer> entry : freqMap.entrySet()){
            if (entry.getValue() == feq){
                list.add(entry.getKey());
            }
        }
        int[] ans = new int[list.size()];
        int j = 0;
        for (int i : list){
            ans[j ++] = i;
        }
        return ans;
    }

    private int transverse(TreeNode root) {
        if (root == null){
            return 0;
        }
        int v = root.val + transverse(root.left) + transverse(root.right);
        Integer i = freqMap.get(v);
        if (i == null){
            i = 0;
        }
        freqMap.put(v, i + 1);
        feq = Math.max(feq, i + 1);
        return v;
    }
}
```
