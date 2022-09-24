---
title: leetcode-124-MaxPathSum
date: 2022-04-24 14:40:52
summary: 二叉树中的最大路径和
categories: leetcode
tags:
- 树
- 最大路径和

---
## MaxPathSum
[leetcode-124-MaxPathSum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)


### 二叉树中的最大路径和

```java
public class MaxPathSum {

    public class TreeNode {
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


  int res = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {

        maxSum(root);
        return res;
    }

    public int maxSum(TreeNode root) {
        if (root == null){
            return 0;
        }
        int left = maxSum(root.left);
        int right = maxSum(root.right);
//        // 仅包含当前节点
//        root.val;
//        // 包含当前节点与左节点
//        root.val + left;
//        // 包含当前节点与右节点
//        root.val + right;
//        // 包含当前节点与左右节点
//        root.val + left + right;

        int m = Math.max(root.val + left, root.val + right);
        res = Math.max(Math.max(Math.max(root.val, root.val + left + right), m), res);

        // 向上层返回 最大者 1.仅包含当前节点，2.包含当前节点与左节点，3.包含当前节点与右节点
        m = Math.max(root.val, m);
        return m;
    }

}
```