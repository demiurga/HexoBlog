---
layout: post
title: '[閒聊] WebClient端的關注點分離'
date: 2014-02-03 10:55
comments: true
categories: 
---
內容出自[Javascript:patterns](http://shop.oreilly.com/product/9780596806767.do)第八章

web的client端可以分成三個主要的關注點，分別是：
* Content(內容，HTML文件)
* Presentation(表現，CSS樣式)
* Behavior(行為，包含使用者互動及對文件的動態改變，通常使用Javascript)

在做client端的規畫時，應該盡可能地保持關注點分離，有助於提升程式的品質及可維護性。 **關注點分離** 同樣可以和 **漸進式增強** 的觀念相輔相成：使用者從最基礎的內容（HTML）開始，隨著支援功能的增強而獲得更好的體驗（CSS, Javascript）。在實際應用中如下：
* 將CSS關閉，確認網頁內容是否可以使用﹑內文是否可以閱讀
* 將Javascript關閉，確保網頁主要功能還是可以正常運作，所有的連結都可以使用（所以不要用`href="#"`），所有的表單都可以正常的提交。
* 不要使用內嵌的事件處理器（例如`onclick`），或是內嵌的style屬性，因為這些東西不屬於內容層
* 撰寫語意化的HTML

Javascript的風格應為 **unobtrusive** ，意思是不應該擋到使用者的路，如果遇到不支援的瀏覽器也不應該讓頁面無法使用。另外，在處理瀏覽器差異時，應該用功能檢測取代瀏覽器偵測。

##一點小感想
---
會寫這篇文章，是因為對書中有些地方有些不同的想法。我同意漸進式增強的觀念，同時也贊同關注點分離帶來的程式品質與可維護性，但是在有些狀況下，完美的關注點分離可能會降低使用者的最高體驗。例如SPA Web，在大量使用ajax的狀況下很難達到＂移除javascript仍然保持網站正常運作＂的條件，但是有必要為了這些極少數不支援javascript的使用者（無障礙網頁另當別論），去放棄大部分使用者的體驗嗎？記得之前看過某篇文章（原諒我找不到他），作者的觀點是＂ **給那些使用者一些提示，並引導他們前進** ＂，我想這個做法不論是對於使用者或是生產者而言都是好的。
> 雖然[IE6,7使用者比例終於下跌了](http://arstechnica.com/information-technology/2013/10/internet-explorer-6-usage-drops-below-5-percent-in-september/)，但是廣大中國網民的IE6比率依舊讓人絕望阿...