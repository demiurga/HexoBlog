title: learning es6 with babel - Class
tags:
---

es6中新增了`Class`關鍵字，它基本上是一顆語法糖，功能跟我們慣用的`Klass`大同小異

```js
class Animal {

  constructor(name) {
    this.name = name;
  }

  run() {
    console.log(this.name + " is running");
  }
}
```
with babal:

```js
"use strict";


function _classCallCheck(instance, Constructor) { 
	// 檢查Animal的實體是不是由Animal建立，避免被當成function用
	// 例如Animal()這種用法, instance會指向window/global(nodejs)
	if (!(instance instanceof Constructor)) { 
		throw new TypeError("Cannot call a class as a function"); 
	} 
}

var Animal = (function () {

	// 這裡面基本上就是es5的klass pattern
  function Animal(name) {
    // 在建立之前先做檢查
    _classCallCheck(this, Animal);

    this.name = name;
  }

  Animal.prototype.run = function run() {
    console.log(this.name + " is running");
  };

  return Animal;
})();
```
再來看看繼承方式:

```js
class A {
  func() {
    
  }
}

class B extends A {
  func() {
    super()
  }
}
```

```js
"use strict";

function _inherits(subClass, superClass) { 
	
	if (typeof superClass !== "function" && superClass !== null) { 
		throw new TypeError("Super expression must either be null or a function, not " + typeof superClass); 
	} 
	
	subClass.prototype = Object.create(
		superClass && superClass.prototype, { 
			constructor: { 
				value: subClass, 
				enumerable: false, 
				writable: true, 
				configurable: true 
			} 
		}
	); 
	if (superClass) subClass.__proto__ = superClass; 
}

function _classCallCheck(instance, Constructor) { 
	if (!(instance instanceof Constructor)) { 
		throw new TypeError("Cannot call a class as a function"); 
	} 
}

var A = (function () {
  function A() {
    _classCallCheck(this, A);
  }

  A.prototype.func = function func() {};

  return A;
})();

var B = (function (_A) {
  function B() {
    _classCallCheck(this, B);

    if (_A != null) {
      _A.apply(this, arguments);
    }
  }

  _inherits(B, _A);

  B.prototype.func = function func() {
    _A.prototype.func.call(this);
  };

  return B;
})(A);
```
