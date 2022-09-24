---
title: leetcode-1146-Snapshot Array
date: 2022-04-30 19:50:52
summary: 快照数组
categories: leetcode
- 二分

---
## 快照数组

[leetcode-1146-Snapshot Array](https://leetcode.cn/problems/snapshot-array/)


```java
class SnapshotArray {

    int snap = 0;
    TreeMap<Integer,Integer>[] arr;
    public SnapshotArray(int length) {
        arr = new TreeMap[length];
    }

    public void set(int index, int val) {
        if (arr[index] == null){
            arr[index] = new TreeMap<>();
        }
        arr[index].put(snap, val);
    }

    public int snap() {
        return snap ++;
    }

    public int get(int index, int snap_id) {
        if (arr[index] == null){
            return 0;
        }
        if (arr[index].get(snap_id) == null && arr[index].floorKey(snap_id) == null){
            return 0;
        }else if (arr[index].get(snap_id) != null){
            return arr[index].get(snap_id);
        }else {
            return arr[index].floorEntry(snap_id).getValue();
        }
    }
}
```
