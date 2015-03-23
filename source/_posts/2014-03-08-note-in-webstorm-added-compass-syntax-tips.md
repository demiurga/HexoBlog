---
layout: post
title: '[筆記] 在WebStorm中新增compass的語法提示'
date: 2014-03-08 14:31
comments: true
categories: 
---
在WebStorm中，compass是沒有內建代碼提示的，而且所有的compass mixin都會被標示為undefined mixin（雖然不影響compiler），連`@import compass`底下都會爬一條紅蚯蚓，跟你說他找不到compass.sass這個檔案。目前看到最好的解法是手動新增符號連結。

先切到專案中的sass目錄裡
```
cd projectlocate/sass
```
新增一個compass的符號連結
```
mklink /d compass $COMPASS_LOCATE
```
$COMPASS_LOCATE`是裝在ruby gem底下的compass stylesheet位置，舉例來說我的長這樣：
```
H:\Units\Ruby193\lib\ruby\gems\1.9.1\gems\compass-0.12.3\frameworks\compass\stylesheets\compass
```
然後就可以得到compass的代碼提示了，這作法就類似把新增compass到你的libary裡面，只是沒有實際引入檔案。

如果要移除這個符號連結， **不要直接刪除**，要用下面這行指令：
```
rmdir compass
```
至於mac或linux中，Symbolic Link的指令是這樣：
```
ln -s <dest> <link>
rm <dest>
```
例如我的是/Library/Ruby/Gems/2.0.0/gems/compass-0.12.3/frameworks/compass/stylesheets/compass



> 我說大哥阿，你既然都bundle compass進去了，為什麼不搞得徹底一點呢....
