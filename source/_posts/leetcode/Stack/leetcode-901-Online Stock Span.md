---
title: leetcode-901-Online Stock Span
date: 2022-06-08 19:45:52
summary: 股票价格跨度
categories: leetcode
tags:
- Min Stack   

---
## 股票价格跨度
[leetcode-901-Online Stock Span](https://leetcode.cn/problems/online-stock-span/)


```java
class StockSpanner {

    LinkedList<Node> stack;

    class Node{
        int val;
        int span;
        Node(int val, int span){
            this.val = val;
            this.span = span;
        }
    }

    public StockSpanner() {
        stack = new LinkedList<>();
    }

    public int next(int price) {

        int sum = 1;
        while (!stack.isEmpty() && stack.getLast().val <= price){
            Node node = stack.pollLast();
            sum += node.span;
        }
        stack.add(new Node(price, sum));
        return sum;

    }
}
```
