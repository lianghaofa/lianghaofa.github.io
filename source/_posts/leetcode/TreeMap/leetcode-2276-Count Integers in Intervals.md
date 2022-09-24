---
title: leetcode-2276-Count Integers in Intervals
date: 2022-06-24 18:05:52
summary: 统计区间中的整数数目
categories: leetcode
tags: 

---
## 统计区间中的整数数目
[leetcode-2276-Count Integers in Intervals](https://leetcode.cn/problems/count-integers-in-intervals/)


```java
public class CountIntervals {


    TreeMap<Integer, Integer> m;
    int cnt;
    public CountIntervals() {
        m = new TreeMap<>();
    }


    public void add(int left, int right) {

        /**
         * Gets the entry corresponding to the specified key; if no such entry
         * exists, returns the entry for the least key greater than the specified
         * key; if no such entry exists (i.e., the greatest key in the Tree is less
         * than the specified key), returns {@code null}.
         */
        // m.ceilingEntry(left)

        /**
         * Gets the entry corresponding to the specified key; if no such entry
         * exists, returns the entry for the greatest key less than the specified
         * key; if no such entry exists, returns {@code null}.
         */
        //m.floorEntry();
        for (Map.Entry<Integer, Integer> e = m.ceilingEntry(left); e != null && e.getValue() <= right; e = m.ceilingEntry(left)) {
            int l = e.getValue(), r = e.getKey();
            left = Math.min(left, l);
            right = Math.max(right, r);
            cnt -= r - l + 1;
            m.remove(r);
        }
        cnt += right - left + 1;
        m.put(right, left);
    }

    public int count() { return cnt; }

}
```
