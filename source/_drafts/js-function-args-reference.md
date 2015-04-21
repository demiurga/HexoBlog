title: js reference小陷阱
tags:
---

前幾天遇到一個有趣的小陷阱，拿出來跟大家分享一下
```js
var list = [{id:1}, {id:2}]

var removeObjInArrById = function(_list, _id){
  _list = _.reject(list, function(f){return f.id === _id})
}

removeObjInArrById(list, 1)

console.log(list) // [{id:1}, {id:2}] ...wtf?
```
上面是一個簡單的`function`，用來移除`list`中對應`id`的物件，不過結果卻失敗了...why?

仔細trace一遍之後，發現原來是reference...我們來看一下問題出在哪：
```js
var list = [{id:1}, {id:2}]
```
建立變數`list`，指向一個匿名陣列`[{id:1}, {id:2}]`，姑且稱之為`xArray`
```js
var removeObjInArrById = function(_list, _id){
  _list = _.reject(list, function(f){return f.id === _id})
}
```
建立一個變數`removeObjInArrById`，指向一個匿名函式...叫他`xFunc`好了
```js
removeObjInArrById(list, 1)
```
摳剛剛建立的`removeObjInArrById()`函式，傳入變數`list`的**參考**，以及一個值為`1`的
number object。接下來就是重點了：
```js
function(_list, _id){
  ...
```
接下來進入`xFunc`，這時local var`_list`指向外部的`list`，而`_id`指向一個新的`Nember(1)`
```js
_list = _.reject(list, function(f){return f.id === _id})
```
`_.reject`會回傳一個新的Array（取名為`arrFilted`），然後讓`_list`指向`arrFilted`
...注意到問題了嗎？

是的，這裡改變的是`removeObjInArrById`的scope中的`_list`的參考，而外部的`list`只是失去一個
參考而已，物件的內容並沒有被改變。
