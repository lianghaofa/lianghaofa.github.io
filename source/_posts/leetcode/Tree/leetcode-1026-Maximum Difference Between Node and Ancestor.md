---
title: leetcode-1026-Maximum Difference Between Node and Ancestor
date: 2022-06-23 14:21:52
summary: 节点与其祖先之间的最大差值
categories: leetcode
tags:


---
## 节点与其祖先之间的最大差值
[leetcode-1026-Maximum Difference Between Node and Ancestor](https://leetcode.cn/problems/maximum-difference-between-node-and-ancestor/)


```java
class Solution {
    public int maxAncestorDiff(TreeNode root) {
          if (root == null){
              return 0;
          }
        return maxAncestorDiff(root,root.val, root.val);
    }


    public int maxAncestorDiff(TreeNode root,int min, int max) {
          if (root == null){
              return 0;
          }
        return Math.max(Math.max(Math.abs(root.val - min), Math.abs(root.val - max)), Math.max(maxAncestorDiff(root.left, Math.min(min, root.val), Math.max(max, root.val)), maxAncestorDiff(root.right, Math.min(min, root.val), Math.max(max, root.val))));
    }
}
```
