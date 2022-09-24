---
title: leetcode-1171-Remove Zero Sum Consecutive Nodes from Linked List
date: 2022-06-17 19:40:52
summary: 从链表中删去总和值为零的连续节点
categories: leetcode
tags:
 - 

---
## 从链表中删去总和值为零的连续节点
[leetcode-1171-Remove Zero Sum Consecutive Nodes from Linked List](https://leetcode.cn/problems/remove-zero-sum-consecutive-nodes-from-linked-list/)



```java
class Solution {
    public ListNode removeZeroSumSublists(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        Map<Integer, ListNode> map = new HashMap<>();

        int sum = 0;
        for (ListNode d = dummy; d != null; d = d.next) {
            sum += d.val;
            map.put(sum, d);
        }

        sum = 0;
        for (ListNode d = dummy; d != null; d = d.next) {
            sum += d.val;
            d.next = map.get(sum).next;
        }

        return dummy.next;
    }
}
```