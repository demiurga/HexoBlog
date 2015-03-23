title: learning es6 with babel - let
date: 2015-03-20 23:29:00
tags:
---

`let`是另一種宣告變數的方式，他有幾個特點：有block scope，沒有hoisting，不能重複宣告
```js
//es6
{
  let foo = "foo";
  var bar = "bar";
}

console.log(foo); //<= Error: foo is not defined
console.log(bar); //<= bar
```

babel對他的實現方式是在前面加個prefix`_`
```js
//babel
var _temporalUndefined = {};
{
  var _foo = _temporalUndefined;
  _foo = "foo";
  var bar = "bar";
}
```
`let`不像`var`一樣具有hoisting的特性，這造成一些現象
```js
//es6
{
  console.log(x); // <=x is not defined - temporal dead zone
  let x = 1;
}
```
在一個block scope中，只要有進行`let`宣告的變數，那麼該變數在被宣告前都不能被使用。
這個區塊就被稱為temporal dead zone
```js
//es6
{
  //-- dead zone for x start --

  console.log(x); // <=ReferenceError: x is not defined - temporal dead zone

  //-- dead zone for x end --
  let x = 1;
  // u can use x now.
  console.log(x); // <= 1

}
```
babel的實現方式如下
```js
"use strict";

var _temporalAssertDefined = function (val, name, undef) {
  if (val === undef) {
    throw new ReferenceError(name + " is not defined - temporal dead zone");
  }
  return true;
};

var _temporalUndefined = {};
{
  var x = _temporalUndefined;

  console.log(_temporalAssertDefined(x, "x", _temporalUndefined) && x);

  x = {};

  console.log(_temporalAssertDefined(x, "x", _temporalUndefined) && x);
}
```
他一開始先在全域宣告一個`_temporalUndefined`物件作為not defined flag，當我們在block scope中做`let`宣告時，
他會先在scope的開始部分就先用`var`宣告物件，然後指向全域的`_temporalUndefined`物件。接下來
在每次調用`x`時，都會先檢查他的參考對象是不是`_temporalUndefined`，如果是的話就拋出
`ReferenceError`

可以注意的點是，`val === undef`比較的是參照，而不是值，所以就算你用`let x={}`也不會怎樣，
因為這兩個`{}`是不同的匿名物件
```js
//es6
{
  console.log(x); // <=ReferenceError: x is not defined - temporal dead zone
  let x = 1;
  console.log(x);
}

//babel
"use strict";

var _temporalAssertDefined = function (val, name, undef) {
  if (val === undef) {
    // 這裡是檢查參照，而不是值
    throw new ReferenceError(name + " is not defined - temporal dead zone");
  }
  return true;
};

//宣告一個匿名物件，指定物件_temporalUndefined參照他
var _temporalUndefined = {};

{
  var x = _temporalUndefined;

  console.log(_temporalAssertDefined(x, "x", _temporalUndefined) && x);
  //宣告一個匿名物件，指定物件x參照他。酸然值相同，但這跟上面那個匿名物件完全無關
  x = {};
  console.log(_temporalAssertDefined(x, "x", _temporalUndefined) && x);
}
```
當撞名時babel會自動改名，不必擔心
```js
//es6
var _temporalUndefined;
{
  let x = {};
}

//babel
"use strict";

var _temporalAssertDefined = function (val, name, undef) { if (val === undef) { throw new ReferenceError(name + " is not defined - temporal dead zone"); } return true; };

var _temporalUndefined2 = {}; //變成_temporalUndefined2了
var _temporalUndefined;
{
  var x = _temporalUndefined2;
  x = {};
}
```
"不能重複宣告"這項特性則是在babel compile階段做檢查的
```js
//es6
{
  let x = 1;
  let x = 2;
}
/* babel compile err
repl: Line 3: Duplicate declaration "x"
  1 | {
  2 |   let x = 1;
> 3 |   let x = 2;
    |       ^
  4 | }
*/
```
