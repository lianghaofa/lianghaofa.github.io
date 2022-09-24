---
title: leetcode-2104-Sum of Subarray Ranges
date: 2022-06-17 20:45:52
summary: 子数组范围和
categories: leetcode
tags:
- Min Stack   

---
## 子数组范围和
[leetcode-2104-Sum of Subarray Ranges](https://leetcode.cn/problems/sum-of-subarray-ranges/)


```java
class Solution {
    public long subArrayRanges(int[] nums) {

        // 所有子数组的最大元素  找两侧更大的元素，此区间的最大值为 当前值

        // 所有子数组的最小元素  找两侧更小的元素，此区间的最小值为 当前值



        return sumSubarrayMaxs(nums) - sumSubarrayMins(nums) ;
    }


    class Node{
        int index;
        int val;
        Node(int index, int val){
            this.index = index;
            this.val = val;
        }
    }

    public long sumSubarrayMins(int[] arr) {

        // 所有子数组的最小元素  找两侧更小的元素，此区间的最小值为 当前值 ， 单调递增
        ArrayDeque<Node> stack = new ArrayDeque<>();
        long ans = 0;
        for (int i = 0; i < arr.length; i ++){
            while (!stack.isEmpty() && arr[i] <= stack.getLast().val){
                Node node = stack.pollLast();
                int l = stack.isEmpty() ? 0 : stack.getLast().index + 1;
                int r = i - 1;
                int index = node.index;
                long c = (index - l + 1) * (r - index + 1) + 1;
                ans += (c * node.val);
            }
            stack.add(new Node(i, arr[i]));
        }
        while (!stack.isEmpty()){
            Node node = stack.pollLast();
            int l = stack.isEmpty() ? 0 : stack.getLast().index + 1;
            int r = arr.length - 1;
            int index = node.index;
            long c = (index - l + 1) * (r - index + 1) + 1;
            ans += (c * node.val);
        }

        return ans;
    }


    public long sumSubarrayMaxs(int[] arr) {

        // 所有子数组的最小元素  找两侧更小的元素，此区间的最小值为 当前值 ， 单调递增
        ArrayDeque<Node> stack = new ArrayDeque<>();
        long ans = 0;
        for (int i = 0; i < arr.length; i ++){
            while (!stack.isEmpty() && arr[i] >= stack.getLast().val){
                Node node = stack.pollLast();
                int l = stack.isEmpty() ? 0 : stack.getLast().index + 1;
                int r = i - 1;
                int index = node.index;
                long c = (index - l + 1) * (r - index + 1) + 1;
                ans += (c * node.val);
            }
            stack.add(new Node(i, arr[i]));
        }
        while (!stack.isEmpty()){
            Node node = stack.pollLast();
            int l = stack.isEmpty() ? 0 : stack.getLast().index + 1;
            int r = arr.length - 1;
            int index = node.index;
            long c = (index - l + 1) * (r - index + 1) + 1;
            ans += (c * node.val);
        }
        return ans;
    }
}
```
