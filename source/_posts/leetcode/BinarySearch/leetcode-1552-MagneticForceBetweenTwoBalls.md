---
title: leetcode-33-Magnetic Force Between Two Balls
date: 2022-04-30 19:50:52
summary: 两球之间的磁力
categories: leetcode
- 二分

---
## 搜索旋转排序数组



### 暴力解

1.先获取所有磁力
2.从最大的磁力开始试  能不能分成 m - 1 段

[leetcode-1552-MagneticForceBetweenTwoBalls](https://leetcode-cn.com/problems/magnetic-force-between-two-balls/)


如何判断是否能分成 m - 1 段 ？  排好序后暴力分割
```
    private static boolean canSeparate(int[] position, int m, int dist){
        // 事前已把 position 排好序
        int count = 0;
        int pre = position[0];
        for (int i = 1; i < position.length; i ++){
            if (position[i] - pre >=  dist){
                pre = position[i];
                count ++;
            }
        }
        return count >= (m - 1);
    }
```
** 只是提供一种暴力思路... 主要还是二分解！ **
```java
    public static int maxDistance1(int[] position, int m) {

        // 1.先获取所有磁力
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        Arrays.sort(position);
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < position.length; i ++){
            for (int j = i + 1; j < position.length; j ++){
                int dist = Math.abs(position[j] - position[i]);
                if (!set.contains(dist)){
                    maxHeap.add(dist);
                    set.add(dist);
                }
            }
        }
        // 2.从最大的开始试  能不能分成 m - 1 段
        while (!maxHeap.isEmpty()){
            int dist = maxHeap.poll();
            if (canSeparate(position, m, dist)){
                return dist;
            }
        }
        return -1;
    }
```

### 二分查找

使用二分查找，得有范围。
左边界 l = 1;
右边界 r = 最大值。 

```java
    public static int maxDistance(int[] position, int m) {

        // 优化解就在 如何找磁力

        Arrays.sort(position);
        int minDist = 1;
        int maxDist = position[position.length - 1];
        int ans = 0;
        while (minDist <= maxDist){
            int mid = minDist + (maxDist - minDist >> 1);
            if (canSeparate(position, m, mid)){
                ans = mid;
                minDist = mid + 1;
            }else {
                maxDist = mid - 1;
            }
        }
        return ans;
    }

    private static boolean canSeparate(int[] position, int m, int dist){
        // 事前已把 position 排好序
        int count = 0;
        int pre = position[0];
        for (int i = 1; i < position.length; i ++){
            if (position[i] - pre >=  dist){
                pre = position[i];
                count ++;
            }
        }
        return count >= (m - 1);
    }
```