---
title: leetcode-1028-Recover a Tree From Preorder Traversal
date: 2022-06-08 18:02:52
summary: 从先序遍历还原二叉树
categories: leetcode
tags:


---
## 从先序遍历还原二叉树
[leetcode-1028-Recover a Tree From Preorder Traversal](https://leetcode.cn/problems/recover-a-tree-from-preorder-traversal/)


```java
public class RecoverFromPreorder {

      public static class TreeNode {
      int val;
      TreeNode left;
      TreeNode right;
      TreeNode() {}
      TreeNode(int val) { this.val = val; }
      TreeNode(int val, TreeNode left, TreeNode right) {
          this.val = val;
          this.left = left;
          this.right = right;
      }
  }


    public static void main(String[] args) {
        recoverFromPreorder("1-2--3--4-5--6--7");
    }

   static int index = 0;

    public static TreeNode recoverFromPreorder(String traversal) {
        index = 0;
          return recover(traversal.toCharArray(), 0);
    }

    private static TreeNode recover(char[] chars, int level){
        int i = index;
        while (i < chars.length && chars[i] == '-'){
            i ++;
        }
        int len = i - index;
        if (len == level){
            int l = i;
            while (i < chars.length && chars[i] != '-'){
                i ++;
            }
            if (i - l <= 0){
                return null;
            }
            TreeNode node = new TreeNode(Integer.parseInt(new String(chars, l, i - l)));
            index = i;
            node.left = recover(chars, level + 1);
            node.right = recover(chars, level + 1);
            return node;
        }
        return null;
    }

}
```
