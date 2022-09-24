---
title: leetcode-958-Check Completeness of a Binary Tree
date: 2022-06-04 16:20:52
summary: 二叉树的完全性检验
categories: leetcode
tags:


---
## 二叉树的完全性检验
[leetcode-958-Check Completeness of a Binary Tree](https://leetcode.cn/problems/check-completeness-of-a-binary-tree/)


```java
class Solution {
    public static boolean isCompleteTree(TreeNode root) {
          if (root == null){
              return true;
          }

        HashMap<Integer,Integer> hashMap = new HashMap<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        hashMap.put(root.hashCode(), 0);
        int index = -1;
        while (!queue.isEmpty()){
            TreeNode treeNode = queue.poll();
            if (hashMap.get(treeNode.hashCode()) != (index + 1)){
                return false;
            }
            index ++;
            TreeNode l = treeNode.left;
            if (l != null){
                queue.add(l);
                hashMap.put(l.hashCode(), index * 2 + 1);
            }

            TreeNode r = treeNode.right;
            if (r != null){
                queue.add(r);
                hashMap.put(r.hashCode(), index * 2 + 2);
            }
        }
        return true;
    }
}
```
