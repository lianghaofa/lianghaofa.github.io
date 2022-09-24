---
title: leetcode-971-Flip Binary Tree To Match Preorder Traversal
date: 2022-07-07 14:40:52
summary: 翻转二叉树以匹配先序遍历
categories: leetcode
tags:


---
## 翻转二叉树以匹配先序遍历
[leetcode-971-Flip Binary Tree To Match Preorder Traversal](https://leetcode.cn/problems/flip-binary-tree-to-match-preorder-traversal/)


```java
class Solution {
    int index = 0;
    public  List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {
        index = 0;
        List<Integer> ans = new ArrayList<>();
        List<Integer> ans1 = new ArrayList<>();
        ans1.add(-1);
        return flip(root,voyage, ans) ? ans : ans1;
    }

    public boolean flip(TreeNode root, int[] voyage, List<Integer> ans) {

        if (root == null){
            return true;
        }
        if (voyage[index] != root.val){
            return false;
        }
        if (index + 1 >= voyage.length){
            return true;
        }
        index ++;
        if (root.left != null && voyage[index] == root.left.val){
            return flip(root.left, voyage, ans) && flip(root.right, voyage, ans);
        }else if (root.left == null && root.right == null) {
            return true;
        }else if (root.left == null && voyage[index] == root.right.val ){
            return flip(root.right, voyage, ans);
        } else if (root.right != null && voyage[index] == root.right.val){
            TreeNode treeNode = root.left;
            root.left = root.right;
            root.right = treeNode;
            ans.add(root.val);
            return flip(root.left, voyage, ans) && flip(root.right, voyage, ans);
        }else {
            return false;
        }
    }
}
```
