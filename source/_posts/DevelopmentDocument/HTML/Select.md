---
title: HTML元素-select
date: 2022-04-29 19:45:52
summary: HTML元素-select的基本操作
categories: HTML
tags:
- select   

---
## HTML元素-select

```html
<select id="u2936Select" class="text " >
</select>
```

**为Select追加一个Option(下拉项)**
```javascript
   $("#u2936Select").append("<option value='"+ 0 +"'>" + "请选择攻击类型" + "</option>");
```

**清除选择框**
```javascript
   $("#u2936Select").empty();
```

**设置Select的选择值**
```javascript
   $("#region_text").val(1)
```

**获取Select的值与内容**
```javascript
$("#u2925Select").val()
$("#region_text").text();
```

**监听Select的值与内容**
```javascript
$("#u2925Select").change(function(){
    var o = countryMap.get(parseInt($("#u2925Select").val()));
    var res = "";
    if (o != null){
        res = o.region_txt;
    }
    $("#region_text").val(1)
    $("#region_text").text(res);
});
```



