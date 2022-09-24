---
title: leetcode-1305-GetAllElements
date: 2022-05-01 01:40:52
summary: 两棵二叉搜索树中的所有元素
categories: leetcode
tags:
- Morris   

---
## 两棵二叉搜索树中的所有元素
> 1.Morris中序常规套路。
> 2.比较大小。
> 时间 ：O(n + m) 空间 O （1）
[leetcode-1305-GetAllElements](https://leetcode-cn.com/problems/all-elements-in-two-binary-search-trees/)


```java
class Solution {
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        
        List<Integer> list = new ArrayList<>();
        TreeNode cur1 = Morris(root1);
        TreeNode cur2 = Morris(root2);
        while (cur1 != null || cur2 != null){
            if (cur1 != null && cur2 != null){
                if (cur1.val > cur2.val){
                    list.add(cur2.val);
                    cur2 = Morris(cur2.right);
                }else {
                    list.add(cur1.val);
                    cur1 = Morris(cur1.right);
                }
            }else if (cur2 != null){
                list.add(cur2.val);
                cur2 = Morris(cur2.right);
            }else {
                list.add(cur1.val);
                cur1 = Morris(cur1.right);
            }
        }
        return list;
    }

    private TreeNode Morris(TreeNode cur){
        TreeNode mostRight;
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
            break;
        }
        return cur;
    }
}
```
