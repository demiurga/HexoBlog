---
layout: post
title: '[筆記] 關於載入Script'
date: 2014-01-17 09:02
comments: true
categories: 
---
最近在看[Javascript Patterns](http://www.tenlong.com.tw/items/9862764198?item_id=427729)這本書，剛好這陣子有碰到比較大的js data，就把書中一些關於Script讀取策略的部分整理一下。

##Script Tag的屬性
---
`language`
這個屬性有各種不同大小寫的格式，像是：`JavaScript`, `javascript`, `Javascript`，有時候還附帶版本號。但其實根本就不應該用這個屬性，因為預設語言就是JavaScript，版本號也運作得不好。
> 註：W3C已經不推薦使用這個屬性

`type`
HTML4跟XHAML1標準會驗證這個屬性為必要，但其實不應該如此，因為預設就是JavaScript。HTML5已經將這個屬性改成非必要。

`defer`
這段指定的Script會在頁面Render結束之後才運作，HTML5有更方便的`async`屬性。
> async跟defer的區別在於前者是在ParseDOM時載入Script並在載入完成後執行，後者則是在頁面載入結束後載入Script，並在載入完成後執行。一個小技巧是同時放上`async`跟`defer`，支援async的瀏覽器會忽略defer，不支援的則會使用defer。
> 關於`defer`這個屬性，其實各瀏覽器實作的方式不一樣，許多現代瀏覽器，例如Chrome，預設就是在頁面Render結束後才載入`<head>`中有`src`的`<scripe>`。[這篇文章](http://mao.li/javascript/javascript-defer/)有作一些測試。

##放置Script Tag的位置
---
`<script>`標籤會阻擋在他之後的事件，包括頁面render或下載，所以放置`<Script>`最好的位置是在`</body>`之前，這樣可以防止頁面載入被Scrtpt標籤阻擋而拖延。最差的方式則是在`<head>`中引入多個獨立的檔案，會佔去server許多不必要的連線數。
> 個人經驗，有沒有pack成單一檔案對伺服器負載能力真的差蠻多的

<!--more-->


##HTTP分塊
---
HTTP協定支援分塊編碼，意思是你可以分批傳送頁面的片段。如果有一個非常複查的頁面，可以用像下面這種方法來分批傳送：
```HTML
<!doctype html>
<html>
<head>
	<title>My App</title>
</head>
<body>
	<div id="header">
  	<!-- content1, like logo -->
  </div>
  // block1 end
  <!-- content2, main content -->
  // block2 end
  <script src="main.js"></script>
</body>
</html>
// block3 end
```
先在第一區塊傳入部分標頭和主體，第二區塊傳入主要內容，第三區塊再傳入js檔為頁面添加特色和互動。這種作法十分符合漸進式增強和unobtrusive javascript的精神。

##動態載入
---
要避免頁面render被script tag拖慢的問題，有下面幾種方法：
* 用XHR請求來下載script，並用`eval()`來執行。不要這樣做。
* 用`defer`和`async`屬性，但是有跨瀏覽器的問題。
* 動態載入`<script>`元素   

最後一種模式是個不錯的方法，像下面這樣：
```js
var script = document.creatElement('script');
script.src = 'Main.js';
document.documentElement.firstChild.appendChild(script);
```
上面簡單的產生了一個script元素，並把他append到`<head>`內。這種方法要注意順序的問題，可以用一個initScriptsArray來解決他：
```js
var initScripts = [],
		init = function(){
    	// for loop initScripts
    };
initScripts.push(function(){
	// some script
});
```
動態載入有很多應用方法，例如把它放到`</body>`前，確保使用者能先確認頁面的靜態內容，並趁這個時候載入互動相關的script，這種方法稱為lazy load（延遲載入）。或是依每個頁面行為或裝置的不同來分塊載入script，例如在mobile上部分較耗資源的效果可能會被停用，這時就可以不必載入那段script，這種方法稱為require load（隨選載入）。也可以在這一頁就預先載入下一頁需要的script，稱為preload（預先載入），不過使用這種方法必須注意，要讓預載的檔案只載入但不分析執行，因為不存在的DOM（下一頁的DOM）可能會造成錯誤。

要下載一段script但不進行分析，可以用`<object>`元素來代替`<script>`：
```js
var obj = document.creatElement('object');
obj.data = 'preload.js';
// 用data屬性代替src
document.body.appendChild('obj');
```
> 書上有提到用image beacon的方式來處理IE，不過我找不到IE跟`<object>`之間發生了什麼事，為什麼要用Image beacon來處理這件事...

預先載入模式可以套用各種類型的元件，例如圖片或影音檔案，主要用在可以預期使用者下一個行為的時候。例如在輸入帳號密碼時，就可以對登入後的頁面進行部分預載。

