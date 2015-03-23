---
layout: post
title: '[筆記] Javascript : The Good Parts - 糟糕的部分'
date: 2013-12-24 08:44
comments: true
categories: [javascript, 筆記]
---
##全域變數
javascript最糟糕的一項特性，就是他對於全域變數的依賴。有三種可以宣告全域變數的方式：
```js
var foo = 'value'; // 直接在最外層宣告
window.foo = 'value' // window物件是所有全域變數的容器
foo = 'value' // 不經宣告就使用變數，稱為implied global
```
第三種原本的用意是對於初學者的體貼（蛤？），但這反而容易造成很難被找出來的bug。
##範圍
如同前面提到的，javascript沒有block scope，只有function scope，而且裡面還有變數抬升的特性。所以最好 **在每個區塊起始的地方宣告完區塊內所有變數** 。
##分號的安插
javascript有個試圖糾正錯誤的機制，它會自動幫你安插分號。不過這個機制有時候會造成更多的麻煩：
```js
return
{
	status: true;
};
```
上面的程式看起來很正常，不過javascript會認為你`return`後面忘了加上分號而貼心的幫你補上，於是就變成這樣：
```js
return; // undefind
{
	status: true;
};
```
所以撰寫javascript時應該採用K&R風格，把`{`放在句尾，可以避免類似的悲劇發生。
<!--more-->

##保留字
作者認為有太多的保留字沒有被使用，造成命名上的麻煩（保留字不能用在變數或特性名稱）
> 不過事實上，有一些是後來實作ECMA5有用到的

##Unicode
在設計javascript的年代，Unicode最多只到65536個字元，但現在已經突破百萬。
javascript的字元是16位元，超出的部分以一對字元表示。Unicode把 **字元對** 當成一個字元，但對javascript來說是兩個。
##typeof
typeof有許多非預期的狀況，例如：
```js
typeof null // object
typeof [1, 2, 3] // object
typeof /a/ // 各瀏覽器實作不一致，可能是object或function
```
##parseInt
`parseInt`是個把字串轉換為整數的函式，他會在遇到非數字字元的時候立即停止，例如：
```js
parseInt('16 say hi~ 17'); // 16
```
這函式還有另一個麻煩，如果字串的第一個字元是0，字串會依照八進位制做轉換，所以最好在每次使用時都帶入進位參數：
```js
parseInt('077'); // 63
parseInt('077', 10); // 77
```
> 這點似乎在新版的Browser有fix過，沒有帶入參數就默認10進位

##+運算子
如果你希望 + 運算子是sum的作用，務必要檢查兩邊是不是"都是"數字。
##浮點數
這是最常被回報的bug。javascript採用[IEEE754標準](http://zh.wikipedia.org/wiki/IEEE_754)，導致他在處理十進位分數的時候會發生一些問題，例如：
```JS
console.log(0.1 + 0.2); // 0.30000000000000004
```
幸好，在整數運算的部分是正確的，所以可以先換成整數做運算在除回來。
##NaN
這也是個奇妙的東西，它的意義是＂非數值(not an number)＂，不過...
```js
typeof NaN === 'number'; // true
```
還有更奇妙的
```js
NaN === NaN // false
NaN !== NaN // true
```
這哪招阿...
幸好javascript有提供一個檢查`NaN`的函式：
```JS
isNaN(NaN); // true
isNaN(0); // false
isNaN('hi~'); // true
isNaN('0'); // false
```
> 這方法有點兩光阿...要檢查NaN還是得判斷兩次才行Orz

還有一個`isFinite`函式，可以判斷＂值是否可以轉成數值＂，他會拒絕`NaN`跟`Infinity`。如果要確認值＂是不是數值(`number`)＂的話，需要自己加工：
```js
var isNumber = function(value){
	return typeof value === 'number' && isFinite(value);
}
```
##偽陣列
前面提過，javascript沒有真正的陣列，而是基於物件的偽陣列。跟真正的陣列相比，他的效能較差，但較容易使用。也因為這個特性的關係，`typeof`運算子沒有辦法區分物件跟陣列。為了判斷是否為陣列，需要把他的Constructor列入考慮：
```js
if(myValue && typeof myValue === 'object' 
		&& myValue.constructor === Array){
  //muValue is Array!
}
```
如果陣列在不同的frame或window下建立，上面的測試可能給出false, 可以改成下面的形式：
```js
if(myValue && typeof myValue === 'object' 
    && typeof myValue.length === 'number'
    && !(myValue.propertyIsEnumberable('length'))){
  //muValue is Array!
}
```
另外，`arguments`並不是個陣列，而是個具有length的物件，但上述測試會把它視為陣列；有時候我們會想要這種結果。
##False類的值
下面這些的值會被歸類在false：
* 0 			 	(number)
* NaN 		 	(number)
*	''			 	(String)
* false 	 	(Boolean)
* null 		  (Object)
* undefind  (undefind)

undefind跟NaN不是常數而是全域變數，可以去更改他們的值。應該不能這樣做，但真的可以。請千萬別這樣做。
> 剛剛試了一下，這好像也會被Browser Fix掉...真可惜（喂！）

##'hasOwnProperty'
`hasOwnProperty`是個方法而非運算子，所以他有可能被覆蓋掉。（應該不會有人這麼白目吧？）

##物件
Javascript的物件從未真正為空，因為他們可以從prototype chain取得成員。下面是一個計算詞彙出現次數的範例:
```js
var i;
var word;
var text = "...some words and constructor"
var words = text.toLowerCase().split(/[\s,.]+/);
var count = {};

for(i = 0; i < words.length; i += 1){
	word = words[i];
  if(count[word]){
  	count[word] += 1;
  } else {
  	count[word] = 1;
  }
}
```

count[constructor]會包含一串很長的字串（chrome是顯示`"function Object() { [native code] }1"`），因為count物件繼承Object.prototype，裡面也有一個constructor特性，他被轉成String然後在尾端加上1。所以應該跟for-in時一樣，用`hasOwnProperty`方法做檢查。