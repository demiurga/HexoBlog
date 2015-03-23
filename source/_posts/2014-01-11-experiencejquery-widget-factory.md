---
layout: post
title: '[心得] jQuery Widget Factory極簡教學'
date: 2014-01-11 08:45
comments: true
categories: 
---


##簡介
jQuery Widget Factory是jQuery UI提供的一個方法，用Factory Mode來建立 **有狀態的** jQuery plugin。大多數的jQueryUI都是用這個方法產生的，例如`button`：
```JS
$.widget( "ui.button", {
	// ...
  });
```
不過也有些是例外，像`datepicker`就不是用這個方法產生的。

##如何使用
這個方法是基於jQuery，而不是jQuery UI Core，但是他的位置是在jQueryui.js裡面，所以兩者都必須引入
> 註：如果不需要其他jQueryUI的話，可以只載入jquery.ui.widget.js

```html
<script type="text/javascript" src="Libs/jquery-1.9.1.js"></script>
<script type="text/javascript" src="Libs/jquery-ui-1.10.3.js"></script>
```
方法的格式如下：
```js
$.widget('namespace.uiName', [base], prototype)
```
`namespace`只能有一層，像jQueryUI就是用`ui.button`
`[base]`可選，用來指定已存在的物件當作基底，預設值是`$.widget`。例如你想要以jqueryUI的`dialog`為基底產生一個新的plugin，就可以像這樣：
```js
$.widget( "custom.superDialog", $.ui.dialog, {
    red: function() {
        this.element.css( "color", "red" );
    }
});
```
`prototype`這個widget的prototype object，套件的方法及設定都在這裡實作。

<!--more-->

##簡單的範例
下面是一個來自官網的簡單的範例，可以在jsBin玩玩看。

首先，建立一個progressbar，然後設定他的初始化方法`_creat()`
```js
$.widget( "custom.progressbar", {
    _create: function() {
        var progress = "%";
        this.element
            .addClass( "progressbar" )
            .text( progress );
    }
});
```
> 還有另一個方法`_init`，兩者間的差異請參考官方文件
> 這裡有另一個重點是`this.element`。如果用一般方法作jquery plugin的話，取得的this是selector後的jquery物件群組，但是widget中的 **this.element永遠只有一個** ，如果你selector後的jquery物件群組有多個物件，他會 **對每個對象各執行一次** 。

為這個pregressbar增加option，設定初始化的值。
```js
$.widget( "custom.progressbar", {
		options: {
    	value: 20%
    },
    _create: function() {
        var progress = this.options.value + "%";
        this.element
            .addClass( "progressbar" )
            .text( progress );
    }
});
```
> `options`是內建屬性，套件的參數會以key-value pair的形式傳到這個物件裡面，在這裡我們給予參數`value`預設值`'20%'`。  

為套件增加public method`getValue`
```js
$.widget( "custom.myprogressbar", {
	options: {
		value: '20'
	},
	getValue: function(){
		return this.options.value;
	},
	_create: function() {
		var progress = this.options.value + "%";
		this.element.text( progress );
	}
});
```
> widget factory提供了很簡單的作法去區分private與public：前面有加`_`的會實作為private，其餘則是public，很方便吧？

接下來，初始化套件並傳遞參數
```html
<div class='progressDiv'></div>
<div class='progressDiv'></div>
```
```js
$('.progressDiv').myprogressbar({ 
	value: 70
});
```
試看看剛剛寫的方法
```js
console.log($('.progressDiv').myprogressbar('getValue')); // 70 * 2
```
完成了！恭喜你，你會製作（簡單的）jQuery plugin了XD
[jsBin Demo](http://jsbin.com/oRIjaVa/1/edit?html,console,output)

##參考文件
[jQuery官網教程](http://learn.jquery.com/jquery-ui/widget-factory/)
[Widget Factory API Document](http://api.jqueryui.com/jQuery.widget/#jQuery-widget1)

##後記
真的有點簡略阿（汗），其實製作過程中還有不少眉眉角角，不過一時之間不好整理出來，有機會再作補充。
