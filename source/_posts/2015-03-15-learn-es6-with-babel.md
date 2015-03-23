title: Learn es6 with babel - Arrows
date: 2015-03-15 22:02:01
tags: learning, es6
---
在ES6中，可以使用`=>`來定義function。這是一個很類似coffeescript的語法糖（事實上es6有很大一部分是語法糖，個人很喜歡這種做法，與其去改變語言的本質，不如讓語言順著原本的脈絡慢慢茁壯）
```js
//es6
var f = (foo) => bar;

//babel
var f = function (foo) {
  return bar;
};
```
當有參數，小括號可以省略
```js
//es6
var f = foo => bar;

//babel
var f = function (foo) {
  return bar;
};
```
當`return`不是單一Expression時，需要用大括號把它包起來，就像一般在寫`function`一樣
```js
//es6
var foo = (v) => {
  if (v === bar)
    play(v);
};

var foo = function (v) {
  if (v === bar) play(v);
};
```
它可以讓code更加的簡短且語意化，例如用在callback上
```js
//js
foo(res => res + 1);

//babel
foo(function (res) {
  return res + 1;
});
```
有種lambda的感覺，對吧？
要注意的是，如同在coffeescript中一樣，fat arrow(`=>`)除了是function的縮寫之外，還具有**在定義時綁定`this`**的特性
```js
//js
var john = {
  _name: "John",
  _friends: ['Mary'],
  printFriends: function() {
    this._friends.forEach(friendName =>
      console.log(this._name + " knows " + friendName));
      //因為上面使用fat arrow的關係, this才會被綁定為"定義時的this"
  }
}

//babel
var john = {
  _name: "John",
  _friends: ["Mary"],
  printFriends: function printFriends() {
    var _this = this;
    //實現方式其實就是我們常在寫的that = this

    this._friends.forEach(function (friendName) {
      return console.log(_this._name + " knows " + friendName);
    });
  }
};

john.printFriends()
// <= John knows Mary

//-- 使用function的版本

//js
var john = {
  _name: "John",
  _friends: ['Mary'],
  printFriends: function() {
    this._friends.forEach(function(friendName){
      console.log(this._name + " knows " + friendName)
      //這裡使用的是function，this會在呼叫時才決定要指向誰
    });
  }
}

john.printFriends()
// if es5 <= undefined knows Mary
// if es6 <= Cannot read property '_name' of undefined
// 在es5中，當this指向全域時會得到window，但在es6會得到undefined
```
