---
layout: post
title: '[筆記] CSS Media Query'
date: 2014-01-23 05:29
comments: true
categories: 
---
最近在做RWD的練習，赫然發現自己的CSS有點弱阿...長久以來都習慣把一些東西交給javascript處理，是時候該打好CSS的根底了。（以上題外話）下面大概筆記一下關於Media Query的部分。

##使用方式
---
有三種使用方式，第一種是在`<link>`Tag中將條件加入：
```html
<link rel="stylesheet" media="screen and (min-width: 450px) and (max-width: 950px)" href="style1.css" />
<link rel="stylesheet" media="screen and (min-width: 950px)" href="style2.css" />
<!-- 當螢幕寬度大於450小於950的時候引入style1，大於950的時候引入style2 -->
```
> `<link>`標籤中的`media`屬性是從html4就有的，只不過那時指定的只有media type，並不支援media query，同樣的，media type在css2中也可以使用。

第二種是在CSS中的`@import`中加入條件：
```css
@import url(color.css) screen and (color);
// 在彩色顯示下引入color.css
```
> 雖然我把他列出來，但請不要使用css的`@import`，他並不是標準的一部分，而且會拖慢網頁的效能。這點在[High Performance Web Sites](http://www.amazon.com/dp/0596529309?tag=stevsoud-20&camp=14573&creative=327641&linkCode=as1&creativeASIN=0596529309&adid=1S1KP4EV129EN37422C0&)中有提到，可以參考[這篇文章](http://www.stevesouders.com/blog/2009/04/09/dont-use-import/)。

第三種是在css selector中加入條件：
```css
@media all and (min-width:500px) { … }
@media (min-width:500px) { … }
// 這兩者是一樣的
```

<!--more-->

##運算子
---
###and
...就是and，既不偉大也不卑微的and
```css
@media tv and (min-width: 700px) and (orientation: landscape) { ... }
```

###逗號
作用等同於`or`，符合其中一個條件的都會套上style
```css
@media (min-width: 700px), handheld and (orientation: landscape) { ... }
```

###not
下面三者的意義是一樣的
```css
@media not all and (monochrome) { ... }
@media not (all and (monochrome)) { ... }
@media (not all) and (monochrome) { ... }
```
`not`並不會影響到逗號之外的判斷式
```css
@media not screen and (color), print and (color)
@media (not (screen and (color))), print and (color)
// 上面兩個是一樣的
```

###only
`only`會對於不支援media query的browser隱藏這份style，支援的劉覽器則會忽略這個字
```html
<link rel="stylesheet" media="only screen and (color)" href="example.css" />
```
> 應該說，不支援Media Querie但正確讀取media type的瀏覽器，會因為media type不是"only"（可能是screen之類的），所以忽略這份樣式。如果browser不支援media type，不管怎樣都會忽略這份style。（如果我沒搞錯的話，連media type都不支援的應該是IE5之前的東西，大概是要用13張3.5吋磁片灌大富翁四的年代...）

##features 
---
請參閱[MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries)。

