---
title: leetcode-812-Largest Triangle Area
date: 2022-04-25 19:40:52
summary: 最大三角形面积
categories: leetcode
tags:
- Math   

---
## 最大三角形面积
[leetcode-812-Largest Triangle Area](https://leetcode.cn/problems/largest-triangle-area/)


```java
public class LargestTriangleArea {


    public static void main(String[] args) {
        int[][] points = new int[5][];
        points[0] = new int[]{0,0};
        points[1] = new int[]{0,1};
        points[2] = new int[]{1,0};
        points[3] = new int[]{0,2};
        points[4] = new int[]{2,0};
        largestTriangleArea(points);
    }

    public static double largestTriangleArea(int[][] points) {
        if (points.length <= 2){
            return 0d;
        }
        int[] startingPoint = null;
        int y = Integer.MAX_VALUE;
        // 选择 y 最小的点
        for (int[] ints : points) {
            if (ints[1] < y) {
                y = ints[1];
                startingPoint = ints;
            }
        }
        List<int[]> hull = new ArrayList<>();
        hull.add(startingPoint);
        int[] prevVertex = startingPoint;
        while (true) {
            int[] candidate = null;
            /*
             * 遍历所有的点
             */
            for (int[] ints : points) {
                if (ints == prevVertex) {
                    continue;
                }
                if (candidate == null) {
                    candidate = ints;
                    continue;
                }
                int ccw = ccw(prevVertex, candidate, ints);
                if (ccw == 0 && dist(prevVertex, candidate) < dist(prevVertex, ints)) {
                    // 共线，选择距离最长的
                    candidate = ints;
                } else if (ccw < 0) {
                    // 不断选择更右的顶点
                    candidate = ints;
                }
            }
            // 回到原点
            if (candidate == startingPoint) {
                break;
            }
            hull.add(candidate);
            prevVertex = candidate;
        }
        double ans = 0d;
        // 任何选择 3 个点
        for (int i = 0; i < hull.size(); i ++){
            int[] a = hull.get(i);
            for (int j = i + 1; j < hull.size(); j ++){
                int[] b = hull.get(j);
                for (int k = j + 1; k < hull.size(); k ++){
                    int[] c = hull.get(k);
                    ans = Math.max(ans, getArea(a, b, c));
                }
            }
        }
        return ans;
    }


    // 知3点，求三角形的面积
    private static double getArea(int[] a, int[] b , int[] c) {
        //                     S = 0.5 * | ab - bc  |
        //                     x0 * y1 +      x1 * y2 +    x2 * y0    - x0 * y2    -  x1  *  y0   - x2 * y1
        double d = Math.abs(a[0] * b[1] + b[0] * c[1] + c[0] * a[1] - a[0] * c[1] - b[0] * a[1] - c[0] * b[1]);
        return 0.5 * d;
    }
    
    static int ccw(int[] a, int[] b, int[] c) {
        /*
         *  二维叉积公式  a × b = |a| |b| sin(θ) n  or  a * b = x1 * y2 - x2 * y1.
         *
         *  原始向量  ab = (b[0] - a[0],b[1] - a[1])   ac = (c[0] - a[0],c[1] - a[1])
         *
         *  ab 与 ac 叉积 = ab * ac = (b[0] - a[0]) * (c[1] - a[1]) - (c[0] - a[0]) * (b[1] - a[1])
  
```
