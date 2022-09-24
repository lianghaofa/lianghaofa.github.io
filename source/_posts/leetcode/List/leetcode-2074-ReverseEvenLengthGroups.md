---
title: leetcode-2074-ReverseEvenLengthGroups
date: 2022-04-26 14:40:52
summary: 反转偶数长度组的节点
categories: leetcode
tags:
- 链表

---
## 反转偶数长度组的节点
[leetcode-2074-ReverseEvenLengthGroups](https://leetcode-cn.com/problems/reverse-nodes-in-even-length-groups/)


> 获取链表的长度
> 1 2 3 4 5 6 ... 2 4 6 ...反转
> 如果最后一组个数是偶数，反转后结束


```java
    public static ListNode reverseEvenLengthGroups(ListNode head) {
        ListNode cur = head;
        int len = 0,index = 0;
        // 获取链表的长度
        while (cur != null){
            cur = cur.next;
            len ++;
        }
        cur = head;
        for (int i = 0; cur != null;){
            int count = i;
            while (count > 0 && cur != null){
                cur = cur.next;
                count --;
                index ++;
            }
            // 如果最后一组个数是偶数，反转后结束
            if (len - index - 1 < i + 1){
                if ((len - index - 1) % 2 == 0){
                    reverseKNode(cur, len - index);
                }
                break;
            }
            // 1 2 3 4 5  cur 分别为 2 , 4 , 6 ... 的前一个节点
            if (i == 0){
                reverseKNode(cur, 2);
                i += 2;
            }else if (i % 2 == 1){
                reverseKNode(cur, i + 1);
                i ++;
            }else {
                i ++;
            }
        }
        return head;
    }

    // 传入前一个指针pre，反转pre后面的k个节点
    private static void reverseKNode(ListNode pre, int k){
        if (pre == null || pre.next == null){
            return;
        }
        ListNode last = pre.next;
        ListNode cur = pre.next.next;
        while (k > 1 && cur != null){
            ListNode next = pre.next;
            last.next = cur.next;
            cur.next = next;
            pre.next = cur;
            cur = last.next;
            k --;
        }
    }
```

### 牛顿迭代