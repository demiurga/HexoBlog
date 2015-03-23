---
layout: post
title: '[筆記] WebStorm中Sass File Watcher的一顆小地雷'
date: 2014-03-08 12:37
comments: true
categories: 
---
當我們在寫Sass的時候，把它拆成各個區塊方便管理是一件很正常的事，像這樣：
```
partial.sass
main.sass
```
然後，因為partial.sass不需要被compiler成partial.css，所以檔名前面會加上加上`_`，變成\_partial.sass。到目前為止一切都很合理。
```
_partial.sass
main.sass
```
接下來在main.sass中引入這個partial，看起來沒什麼問題，不過接下來就是地雷了
```css
@import partial

// main.sass
```
通常，我們會希望在更改\_partial中的內容後，重新編譯引入他的檔案，也就是main.sass。爬過stackoverflow之後，可以知道WebStorm提供了一個選項給你，叫做"track only root files"，取消的話她就會連帶編譯跟這個檔有關的所有檔案，很貼心對吧？

...才怪，當你實際測試之後會發現，更改\_partial.sass的內容後，並沒有編譯出新的main.css，要把import的檔名改一下：
```css
@import _partial

// main.sass
```
這樣子，\_partial.sass的內容被更改的時候，就會一併編譯main.sass了。不過問題還沒結束，仔細看資料夾，你會發現多了一個\_partial.css...沒錯，他連`_`開頭的檔案也編譯了。

正確的解法應該是， **要勾選track only root files，然後import的檔名必須加上`_`**，這樣就能正確的編譯且不產生多餘的檔案了。

> ...馬德，stackoverflow誤我半天...