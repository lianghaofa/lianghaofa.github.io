---
title: leetcode-841-CanVisitAllRooms
date: 2022-04-22 02:40:52
summary: 钥匙和房间
categories: leetcode
tags:
- 图
- DFS

---
## LargestPerimeter
[leetcode-841-CanVisitAllRooms](https://leetcode-cn.com/problems/keys-and-rooms/)

// 需要注意的点
最初，除 0 号房间外的其余所有房间都被锁住。
**0 号房间是打开的**
``` java
    public static boolean canVisitAllRooms(List<List<Integer>> rooms) {

        boolean[] visited = new boolean[rooms.size()];
        dfs(visited, rooms, 0);
        visited[0] = true;
        for (int i = 1; i < visited.length; i ++){
            if (!visited[i]){
                return false;
            }
        }
        return true;
    }

    private static void dfs(boolean[] visited, List<List<Integer>> rooms, Integer cur) {
        if (visited[cur]){
            return;
        }
        List<Integer> roomList = rooms.get(cur);
        visited[cur] = true;
        for (int j = 0; j < roomList.size(); j ++){
            if (visited[roomList.get(j)]){
                continue;
            }
            dfs(visited, rooms, roomList.get(j));
        }
    }
```