---
layout: post
title: '[筆記] Javascript Image Beacon'
date: 2014-02-03 10:22
comments: true
categories: 
---
在javascript中，要對Server發出GET請求，除了慣用的ajax之外還有另外一個方法，叫做Image Beacon。方法如下：
```javascript
var beacon = new Image(),
    url = '/record.asp?',
    params = [
    	'name=John', 'age=18'
    ];
beacon.scr = url + params.join('&');
```
利用`Image`物件的`src`屬性來對伺服器發出GET請求，上面的範例對`/record.asp`發出了一個GET請求，並帶入參數`name`跟`age`。這個Image物件並不需要被Render到頁面中。

Image Beacon跟ajax的差別在於：
* 可跨域
* 效能比XHR來的好
* 只能使用GET，所以有長度限制

這種方法常被用於 **只需要向Server發送數據** 的場合，例如蒐集統計數據。在這種情況下，Server可以回傳一個`204 No Content`的header，代表有收到這份訊息，避免客戶端持續等待。