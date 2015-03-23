---
layout: post
title: '[筆記] Javascript : The Good Parts - 不良的部分'
date: 2013-12-31 09:23
comments: true
categories: [javascript, 筆記]
---
這章列出javascript一些可以輕鬆避用的問題功能。
##==
用`===`或`!==`來取代`==`或`!=`，因為後者在運算子兩邊型別不同時，會強制改變值的型別，而且規則十分複雜，下面列出一些有趣的例子：
```js
'' == '0' // false
0 == '' // true
0 == '0' // true

false == 'false' // false
false == '0' // true

false == undefined // false
false == null // false
null == undefined // true
```
詳細規則可以參考[這張](http://zero.milosz.ca)表，不過不要真的去背他阿（笑）
##with敘述
[with敘述](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with)提供了取用物件特性時的捷徑，但他的結果有時難以預料，因此應該避免使用他。作者認為with光是出現在語言中就已經拖慢了javascript的速度，這個語言沒有他會更好。
##eval
＂eval is evil＂這句耳熟能詳的話就是作者說的。他認為`eval`有以下的問題：

* 難以閱讀
* 拖慢效能
* eval文段有太多權限，影響程式安全性

基於上面這些問題，應該避免使用`eval`。`Function`建構式是`eval`的另外一種形式，同樣應該避免使用。`setTimeout`與`setInterval`函式可接受字串或函式作為引數，使用字串作為引數時，他們的行為就像`eval`，所以使用這兩個函式時應該避免以字串作為引數。
> 關於'eval is evil'有不少的討論。部分的人認為應該在＂合理的需要＂而且＂你知道自己在幹什麼＂的情況下使用他，而不是如此斷定的禁用他。不過我還是傾向於後者，因為＂知道自己在幹什麼＂聽起來很簡單，事實上很難啊

<!--more-->

##continue
`continue`會跳回迴圈的起始處。我還沒見過移除continue敘述後，沒有獲得改善的程式碼。
> 這是作者說的，不是我說的

##switch的案例掉落
在javascript中，`switch-case`敘述在沒有break的情況下，會掉向下一個case：
```js
var a = 'a';
switch(a){
  case 'a':
    document.write(' case1');
  case 'b':
    document.write(' case2');
    break;
  case 'c':
    document.write(' case3'); 
}
// case1 case2
```
這有時候會被當作一個trick，但作者認為＂程式語言裡最糟的功能，不是那些顯然危險或無用的功能，而是那些有用的、具有吸引力但又危險的功能。＂
> 小插曲：其實作者之前也認為fall through的優點足以掩蓋缺點，直到他某天修改JSLint的Bug時膝蓋中了一箭...

##無區塊的敘述
不要為了省兩個字元捅自己或你的隊友。保持一致性有助於把事情做對。

##++跟--
在jsLint中這叫over trick。作者認為這會鼓勵一種魯莽草率的風格，大部分的buffer overrun都來自於這種風格的原始碼。

##Bitwise運算子
在JAVA中，bitwise運算子與整數一起工作，但javascript中沒有整數，只好把double轉成整數處理完後再轉回去。在多數語言中，bitwise離硬體很近而非常快速，但在javascript中卻不是這樣。

##function敘述與function運算式
javascript中同時擁有function敘述(function expression)與function運算式(function declaration)，下面兩者的意義是相等的：
```js
function foo() {}; // function declaration
var foo = function foo() {}; // function expression
```
作者認為後者的形式比較好，可以清楚的表現出＂函式也是個物件＂這個性質。此外，在if敘述中禁止使用function敘述，但多數的瀏覽器沒有理會這項規則，實作方式又各不相同，因而產生了可攜性的問題。

##typed wrapper
javascript有一組type wrapper，例如：
```js
var a = new Boolean(false);
a.valueOf() // false
```
會產生一個有`valueOf`方法的物件，此方法回傳wrapper value。但事實上完全不需要這個功能，不要使用`new String``newNumber``new Boolean`這些方法，也避免使用`new Object`和`new Array`，改用`{}`與`[]`。
##new
new運算子會建立繼承運算元的prototype成員的新物件，然後呼叫運算元，把新物件繫結至this。但是如果你忘了加new，運算元的this會指向window，這個問題無法被編譯器檢測出來，只能靠命名規則來協助。更好的方法是不用new。（這個問題在繼承模式的部分討論過，請參考[這裡](http://apolkingg8.blogspot.com/2013/12/javascript-good-parts-ch5.html)）
##void
在javascript中，void是個運算子，接受運算元並回傳undefined。這功能沒有用。
> 學長，請容許我強調一遍，完全沒有用。