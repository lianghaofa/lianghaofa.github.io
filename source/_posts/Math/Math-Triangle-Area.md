---
title: 平面坐标知3点，求面积
date: 2022-05-15 17:38:52
summary: 平面坐标知3点，求面积
categories: Math
tags:

---
## 平面坐标知3点，求面积


![Math-Triangle-Area](/medias/Math/1652609663.png)

```java
// 知3点，求三角形的面积
    private static double getArea(int[] a, int[] b , int[] c) {

        // S = 0.5 * | ab - bc  |
        //                     x0 * y1 +      x1 * y2 +    x2 * y0    - x0 * y2    -  x1  *  y0   - x2 * y1
        double d = Math.abs(a[0] * b[1] + b[0] * c[1] + c[0] * a[1] - a[0] * c[1] - b[0] * a[1] - c[0] * b[1]);
        return 0.5 * d;
    }
```


