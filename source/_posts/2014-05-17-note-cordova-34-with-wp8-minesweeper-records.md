---
layout: post
title: '[筆記] Cordova 3.4 with WP8 踩地雷紀錄'
date: 2014-05-17 05:52
comments: true
categories: 
---
這整個過程讓我想到WOW裡的一個成就： **［這真是段漫長又奇妙的旅程］**
不過漫長奇妙之外還多了很多的痛苦就是了...ORZ
###1. `cordova platform add wp8`時出現亂碼錯誤
...亂碼是要我怎麼debug阿大哥...
不過還好下面有stack，可以看到問題出在`wp8_parser.js`這隻裡面的這段：
```js
child_process.exec(command, function(err, output, stderr) {
  events.emit('verbose', output);
  if (err) {
  	d.reject(new Error('Requirements check failed: ' + output + stderr));
  } else {
  	d.resolve();
	}
});
```

恩，還是沒有頭緒，試著把err catch註解掉，會得到另一個error`副檔名 .js 沒有對應的 script 引擎`
於是按照google大神的指示，裝了Windows Scrip，然後再把原本的err catch加回去...就可以了。
> 老實說小弟才疏學淺完全不知道發生了什麼事...懇請前輩們指點

###2. `cordova platform add wp8`成功，但是在CREAT SUCCESS之後，要加入plugin時發生錯誤`Non-white space before first tag`
原因出在`\platforms\wp8\CordovaWP8AppProj.csproj`跟`\platforms\wp8\Properties\WMAppManifest.xml`這兩隻檔案一開始的宣告出現亂碼。
這是個尚未被解決的Bug，可以參考[這篇](https://issues.apache.org/jira/browse/CB-6301)
雖然提出者的環境是簡體中文，但是最底下的解法在繁中的環境下依然有效：
> wei jiang added a comment - 09/Apr/14 14:26
I have found a solution for this issue:
1、open C:\Users\.cordova\lib\wp\cordova\3.4.0\wp8\bin\create.js
line 68:
change 
'var f=fso.OpenTextFile(filename,1,2);'
to 
'var f=fso.OpenTextFile(filename,ForReading,false,TristateFalse);'
line 75:
change 
'var f=fso.OpenTextFile(filename, ForWriting, TristateFalse);'
to 
'var f=fso.OpenTextFile(filename, ForWriting,true, TristateFalse);'
2、Change the encoding of these files to 'UTF-8 without BOM' 
in C:\Users\.cordova\lib\wp\cordova\3.4.0\wp8\template
App.xaml
App.xaml.cs
CordovaWP8AppProj.csproj
CordovaWP8Solution.sln
MainPage.xaml.cs
Properties\WMAppManifest.xml
ps:I did this using Notepad++

另外，[這篇](http://my.oschina.net/arrowing/blog/181476#OSC_h2_14)也有另一種做法，不過可能是系統語言不同的關係，我用這種作法會沒辦法正確的parse xml。