---
title: leetcode-621-LeastInterval
date: 2022-04-27 20:40:52
summary: 任务调度器
categories: leetcode
tags:
- 贪心   

---
## 任务调度器
[leetcode-621-LeastInterval](https://leetcode-cn.com/problems/task-scheduler/)


![任务调度器](https://isblog.oss-cn-guangzhou.aliyuncs.com/LEETCODE/LeastInterval-leetcode-621.jpg)

```java
public class LeastInterval {

    public static void main(String[] args) {
        leastInterval(new char[]{'A','A','A','A','A','A','B','C','D','E','F','G'}, 2);
    }

    public static int leastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        int max = 0;
        int sum = tasks.length;
        int c = 0;
        for (char task : tasks) {
            c = task - 'A';
            count[c] ++;
            max = Math.max(max, count[c]);
        }
        int maxCount = 0;
        for (int value : count) {
            if (value == max) {
                maxCount++;
            }
        }
        int time = (max - 1) * (n + 1) + maxCount;
        if (sum > time){
            time += sum - time;
        }
        return time;
    }
}
```
