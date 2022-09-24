---
title: leetcode-655-Print Binary Tree
date: 2022-06-04 16:40:52
summary: 输出二叉树
categories: leetcode
tags:


---
## 输出二叉树
[leetcode-655-Print Binary Tree](https://leetcode.cn/problems/print-binary-tree/)


```java
class Solution {
    public List<List<String>> printTree(TreeNode root) {

        List<List<String>> ans = new ArrayList<>();
        int rows = getDepth(root);
        int c = (int) Math.pow(2, rows - 1);
        int cols = (c - 1) * 2 + 1;
        String[][] chars = new String[rows][cols];
        write(root, 0, cols,0 ,chars);
        for (int i = 0; i < rows; i ++){
            List<String> list = new ArrayList<>();
            for (int j = 0; j < cols; j ++){
                if (chars[i][j] == null){
                    list.add("");
                }else {
                    list.add(chars[i][j]);
                }
            }
            ans.add(list);
        }
        return ans;
    }

    public void write(TreeNode root, int l, int r, int rows ,String[][] chars) {
          if (root == null){
              return;
          }
          int mid = l + (r - l >> 1);
        chars[rows][mid] = String.valueOf(root.val);
        write(root.left, l, mid - 1,rows  + 1, chars);
        write(root.right,mid + 1, r, rows + 1 , chars);
    }
    
    private int getDepth(TreeNode root){
          if (root == null){
              return 0;
          }
          return Math.max(getDepth(root.left), getDepth(root.right)) + 1;
    }
}
```
