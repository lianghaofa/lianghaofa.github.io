---
title: leetcode-965-Univalued Binary Tree
date: 2022-05-24 12:40:52
summary: 单值二叉树
categories: leetcode
tags:
- Morris   

---
## 单值二叉树


[leetcode-965-Univalued Binary Tree](https://leetcode.cn/problems/univalued-binary-tree/)


```java
class Solution {
    public boolean isUnivalTree(TreeNode root) {
        if (root == null){
            return true;
        }
        int num = root.val;
        TreeNode cur = root;
        TreeNode mostRight = null;
        while (cur != null){
            mostRight = cur.left;
            if (mostRight != null){
                while (mostRight.right != null && mostRight.right != cur){
                    mostRight = mostRight.right;
                }
                if (mostRight.right == cur){
                    mostRight.right = null;
                }else {
                    mostRight.right = cur;
                    cur = cur.left;
                    continue;
                }
            }
            if (num != cur.val){
                return false;
            }
            cur = cur.right;
        }
        return true;
    }
}
```
