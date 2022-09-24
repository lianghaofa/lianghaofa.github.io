---
title: Canvas
date: 2022-05-05 20:45:52
summary: canvas的基本使用
categories: Canvas
tags:
- Canvas
---
## Canvas 基本操作

### Creating and Resizing Your Canvas

``` html
    <canvas></canvas>
```


``` javascript
var canvas = document.querySelector('canvas');

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
```


### Drawing Elements
// line
c.beginPath();
c.moveTo(50, 300);
c.lineTo(300,100);
c.lineTo(400,300);
c.strokeStyle = "#891e1e"
c.stroke()

// Arc
c.arc(x, y, 30, 0, Math.PI * 2, false)
c.strokeStyle = "#23b326"
c.stroke()


### Animating Elements 




### Interacting with Elements
