---
title: AJAX
date: 2022-04-29 20:45:52
summary: AJAX的基本操作
categories: AJAX
tags:
- ajax
---
## AJAX的基本操作

```javascript
$.ajax({
    type : "post",
    dataType : "html",
    url : '',
    data : '',
    async : true,
    success : function (res) {

    },
    error : function (res) {
        
    }
})
```