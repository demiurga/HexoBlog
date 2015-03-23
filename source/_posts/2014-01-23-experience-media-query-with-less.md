---
layout: post
title: '[筆記] Media query with LESS'
date: 2014-01-23 07:08
comments: true
categories: 
---
在LESS中，對於Media Query有些更方便的寫法，下面舉一些例子：

##嵌套
---
可以直接把`@media`當成selector寫，例如：
```css
.one {
    @media (width: 400px) {
        font-size: 1.2em;
        @media print and color {
            color: blue;
        }
    }
}
```
會被編譯成
```css
@media (width: 400px) {
  .one {
    font-size: 1.2em;
  }
}
@media (width: 400px) and print and color {
  .one {
    color: blue;
  }
}
```

<!--more-->


##變數化
---
在1.4.0之後的版本，開啟嚴格模式的情況下，可以在Media Query中插入變數：
```css
@media screen, (max-width: @width) { ... }
```
你也可以把media query變數化，例如：
```css
@singleQuery: ~"(max-width: 500px)";
@media screen, @singleQuery {
    set {
        padding: 3 3 3 3;
    }
}
```
會被編譯成
```css
@media screen, (max-width: 500px) {
    set {
        padding: 3 3 3 3;
    }
}
```
> 變數必須是一段完整的media query，像這樣會出錯：`@media screen and @partial { ... }`

變數化的Media Query中也可以插進變數：
```css
@phoneValueMax: ( 599 / @bfs ) + 0em;
@phone: ~"screen and (max-width: @{phoneValueMax} )";
```
##Mixin
---
也可以把它編成Mixin：
```css
@highdensity: ~"only screen and (-webkit-min-device-pixel-ratio: 1.5)",
              ~"only screen and (min--moz-device-pixel-ratio: 1.5)",
              ~"only screen and (-o-min-device-pixel-ratio: 3/2)",
              ~"only screen and (min-device-pixel-ratio: 1.5)";
```
##參考
---
[LESS](http://www.lesscss.org/)
[Variable media queries in Less CSS](http://blog.scur.pl/2012/06/variable-media-queries-less-css/)

