---
layout: post
title: '[筆記] HTML5 Semantic Elements'
date: 2014-02-04 07:59
comments: true
categories: 
---
<img src="http://html5doctor.com/downloads/h5d-sectioning-flowchart.png"/>

其實舊的HTML4.01中也有一些元素是具有語意性的，例如`<form>`、`<table>`、`<img>`，而在HTML5中，新增了更多的常用語意性元素，用來取代以往由各種`<div>`切成的區塊，例如`<head>`、`<footer>`等等。下面整理一些常用的HTML5語意性元素。

###\<header>
用來表示區塊標題的區塊元素，這個區塊可以是一整個頁面、一個段落或一篇文章，可以把它當成一個放置介紹內容的容器。一個頁面中可以有多個header。\<header>是一容器沒錯，但是它裡面裝的東西應該只有區塊的標題或者摘要。不要把\<header>當成\<div class="header">來使用。另外，\<header>也不能放在\<footer>、\<address>或另一個\<header>裡面。

###\<nav>
導覽列。裡面裝的東西應該只有主要的navigation links，不要把各種link都丟到\<nav>裡面。舉例來說，footer裡面常常會有一排link，那個就不需要包進\<nav>。

###\<section>
文件中的一個專題群組或區塊。一般來說，裡面都會包含heading。如果這個區塊的內容可以分成幾個部分的話，那應該使用article。
> Examples of sections would be chapters, the various tabbed pages in a tabbed dialog box, or the numbered sections of a thesis. A Web site’s home page could be split into sections for an introduction, news items, and contact information.

使用section的地方像是文章中的章節，一個標籤式對話框中的各個標籤頁面，或論文的編號部分。一個網站的主業通常可以分成幾個section，像是introduction，news items還有contact information。
> Do not use the \<section> element as a generic container; this is what \<div> is for, especially when the sectioning is only for styling purposes.

不要把section當成div用，這大概是最常見的錯誤用法。section內裝的應該是有意義且附帶標題的一段內容。例如這樣：
```html
<section>
  <h1>Heading</h1>
  <p>Bunch of awesome content</p>
</section>
```

<!--more-->


###\<article>
一個獨立的區塊，同樣必須帶有heading。舉例來說，像這篇文章本身就是一個article，下面每個回應也都是一個單獨的article。
article跟section的區分是，article有更高的獨立性及完整性。MDN裡面是這樣說明的：
> "The HTML \<article> Element represents a self-contained composition in a document, page, application, or site, which is intended to be independently distributable or reusable, e.g., in syndication."

article本身就算脫離了整體也是一個可以獨立存在、具有完整內容的區塊，例如這篇文章；而section雖然也具有獨立表達內容的能力，但是對外層有一定的相依性，例如這篇文章中的一個章節。

###\<aside>
aside元素代表從內容分離的部分。這些部分通常被表示為sidebar或interns。他們通常包含side explanations，像是術語定義；也可以放較為鬆散相關的東西，像廣告、的作者的傳記、個人資料信息或相關連結。像w3school舉的這個例子：
```html
<p>My family and I visited The Epcot center this summer.</p>

<aside>
  <h4>Epcot Center</h4>
  <p>The Epcot Center is a theme park in Disney World, Florida.</p>
</aside>
```
另外，不要用aside標記括弧內的文字，這通常被認為是主要內容的一部分。

###\<footer>
footer代表一個區塊的結尾訊息，這個區塊是離他最近的父 \<article>, \<aside>, \<nav>, \<section>, \<blockquote>, \<body>, \<details>, \<fieldset>, \<figure>, \<td>。footer內通常會包含作者、版權等資訊。footer元素不是父區塊的內容之一，所以並不會出現在outline中。在address元素中的作者資訊也可以放在footer裡面。
```html
<footer>
  <p>Posted by: Hege Refsnes</p>
  <p>Contact information: <a href="mailto:someone@example.com">
  someone@example.com</a>.</p>
</footer>
```

###\<figure>
這也是一個常被誤用的標籤，頻率大概僅次於section。先看看它的定義：
> The figure element represents a unit of content, optionally with a caption, that is self-contained, that is typically referenced as a single unit from the main flow of the document, and that can be moved away from the main flow of the document without affecting the document’s meaning.

簡單來說，figure是一個有完整內容的區塊，他是主要內容的一部分，而且可以任意移動位置而不影響整體內容的表達。常見的問題是把每個img都包上figure，這完全沒有意義。簡單的判斷方式是想想看＂我把這個figure拿掉會不會影響到上下文的閱讀？＂，如果會的話，他就絕對不該是一個figure。
figure絕對不是只拿來包圖片的，他可以包含影音檔、圖表（可能是canvas或是svg）或是一段code。他跟aside的差別在於：

* aside和主內容有關，但不是主內容的一部分
* figure是主內容的一部分，但是他可以任意移動或刪除而不影響主內容的表達。

通常figure會搭配\<figcaption>服用，他放在第一個或最後一個子元素，像這樣：
```html
<figure>
  <img src="/macaque.jpg" alt="Macaque in the trees">
  <figcaption>A cheeky macaque, Lower Kintaganban River, Borneo. Original by <a href="http://www.flickr.com/photos/rclark/">Richard Clark</a></figcaption>
</figure>
```
範例可以參考[HTML5 Doctor](http://html5doctor.com/the-figure-figcaption-elements/)

##其他
---
* IE8以下不支援HTML5，可以用[HTML5shiv](https://code.google.com/p/html5shiv/)來補足。
* 如果沒有十足的把握，可以先用一般的寫法完成頁面，然後再去做語意化的動作。不當的語意化對SEO來說是個悲劇。
* XDite大大在[如何設計出正確語意的 HTML5](http://wp.xdite.net/?p=3071)有提到＂不要對任何語意TAG下style，否則當需要調整語意結構時，會天下大亂＂，可能是我太菜，不知道為什麼會這樣說，也找不到相關的文章。如果每個semantic tag外面都包個div，這樣的code會比較好嗎？

##參考
---
[MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
[W3School](http://www.w3schools.com/html/html5_semantic_elements.asp)
[HTML5Doctor](http://html5doctor.com/)
[Blog.XDite.net:如何設計出正確語意的 HTML5](http://wp.xdite.net/?p=3071)
[避免常見的六種 HTML5 錯誤用法](http://waterlily-lsl.com/modules/article/view.article.php/c1/258)
[HTML5 中 div section article 的区别](http://www.qianduan.net/html5-differences-in-the-div-section-article.html)