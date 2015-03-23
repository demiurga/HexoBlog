title: learning es6 with babel - const
date: 2015-03-20 23:29:12
tags:
---

`const`用來聲明常量，也就是該變數的值在聲明後不會再改變，跟`let`一樣有block scope，沒有
hoisting
```js
//es6
const PI = 3.14159;
// 按照js的naming rule, 常量應該全部大寫
console.log(PI); // <= 3.14159

PI = 3;
console.log(PI); // <= 3.14159

const PI = 3;
console.log(PI); // <= 3.14159
```
babel會在compile階段就檢查`PI`是不是在宣告後有試圖去改變他，所以上面這段會出現Error
```
repl: Line 6: "PI" is read-only
  4 | console.log(PI); // <= 3.14159
  5 |
> 6 | PI = 3;
    | ^
  7 | console.log(PI); // <= 3.14159
  8 |
  9 |
```
由於常量的特性是在compile階段檢查的，所以babel對於`const`其餘特性(block scope,
  no hoising)的實現方式就跟`let`一樣
```js
//es6
{
  const PI = 3.14159;
}

//babel
"use strict";

var _temporalAssertDefined = function (val, name, undef) {
  if (val === undef) {
    throw new ReferenceError(name + " is not defined - temporal dead zone");
  }
  return true;
};

var _temporalUndefined = {};
{
  var PI = _temporalUndefined;
  PI = 3.14159;
}
```
要注意的是`const`不等於`Object.freeze()`：`const`是固定變數的參考，`Object.freeze()`
則是固定變數參考對象的值
```js
const foo = {};

foo.prop = 123;
console.log(foo.prop); // <= 123

var bar = {};
Object.freeze(bar);

bar.prop = 123;
console.log(bar.prop); // <= undefined
```
