---
title: leetcode-148-Sort List
date: 2022-05-08 21:40:52
summary: 排序链表
categories: leetcode
tags:
- 链表

---
## 排序链表
[leetcode-148-Sort List](https://leetcode-cn.com/problems/sort-list/)

> 迭代版归并排序


```java
class Solution {
    public static ListNode sortList(ListNode head) {

        int len = 0;
        ListNode cur = head;
        while (cur != null){
            len ++;
            cur = cur.next;
        }
        int l = 1;
        ListNode pre = new ListNode(Integer.MIN_VALUE);
        pre.next = head;
        while (l <= len){
            ListNode p = pre;
            ListNode s = head;
            ListNode f = pre.next;
            int count = 1;
            while (count * l <= len && f != null){
                s = f;
                int ll = l;
                while (ll > 0 && s != null){
                    s = s.next;
                    if (f != null && f.next != null){
                        f = f.next.next;
                    }else {
                        f = null;
                    }
                    ll --;
                }
                p = MergeSort(p,p.next, s,f);
                count ++;
            }
            l *= 2;
        }
        return pre.next;
    }

    private static ListNode MergeSort(ListNode pre, ListNode cur, ListNode midNode, ListNode lastNode){

        ListNode mid = midNode;
        while (cur != mid && midNode != lastNode){
            if (cur.val > midNode.val){
                ListNode next = midNode.next;
                pre.next = midNode;
                pre = pre.next;
                midNode = next;
            }else {
                ListNode next = cur.next;
                pre.next = cur;
                pre = pre.next;
                cur = next;
            }
        }
        while (cur != mid){
            ListNode next = cur.next;
            pre.next = cur;
            pre = pre.next;
            cur = next;
        }
        while (midNode != lastNode){
            ListNode next = midNode.next;
            pre.next = midNode;
            pre = pre.next;
            midNode = next;
        }
        pre.next = lastNode;
        return pre;
    }
}
```