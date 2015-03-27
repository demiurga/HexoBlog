title: learning es6 with babel - Class
tags:
---
es6中新增了`Class`關鍵字...先別緊張也別興奮，這不是什麼語言本質上的變革，它基本上只是一顆語法糖，
功能跟我們慣用的`Klass`大同小異
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
