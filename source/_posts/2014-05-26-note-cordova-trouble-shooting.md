---
layout: post
title: 'Cordova trouble shooting'
date: 2014-05-26 01:29
comments: true
tags: [Cordova]
---
##開發環境：
Editor: WebStorm 7 with Cordova CLI
SDK: Cordova 3.4.0 / 3.5.0, Xcode 5.1.1, ADT 22.3.0
OS: Mac OSX 10.9.2
Device: iPhone 4S(iOS 7), Butterfly(Android 4.4), Samsung S3(Android 4.3), Samsung Note 2(Android 4.3)

雖然寫這篇文的時間點，官網上的stable版本依然是3.4.1，不過npm上已經有3.5.0了。個人建議可以直接昇到3.5.0，因為有些bug已經在這版解決了，省得你自己去做solution。

---
##1. Camera在android一直出現`Class not defind`，可是在iOS卻好好的，而且我確定該死的config.xml沒有寫錯，這哪招？
A: 先用`cordova plugin ls`檢查一下你的camera plugun叫什麼名字。如果是`com.apache.cordova.camera`的話，接著檢查你的config.xml...我是說`android/res/xml`裡面那隻，看看camera那段，應該是要長這樣：
```xml
<feature name="Camera">
	<param name="android-package" value="org.apache.cordova.camera.CameraLauncher" />
</feature>
```
注意，是`org.apache.cordova.camera.CameraLauncher`，不是`org.apache.cordova.CameraLauncher`。如果你出現的是後者，回到通用的那隻config.xml，把`<feature name="Camera">`整個區塊刪掉，重新rm/add plarform一次。

##2. 在android 4.4，使用`camera.getPicture()`方法抓取檔案中的圖片，發生一些奇怪的事：同一張圖片從不同路徑進去點選，有些路徑抓的到（例：相簿），有些路徑卻抓不到（例：最近使用的檔案），why?
A: 這是android 4.4的新改變造成的，詳情可以看這篇issue：[Pick image from Library or Photo album on android 4.4](https://issues.apache.org/jira/browse/CB-5398)，簡單來說就是uri的形式改變了。解決方法有兩種，一種是去改CameraLauncher.java，自己加入kitkat的hack；另外一種是把cordova昇到3.5.0版。對懶人小弟我來說，選哪種很明顯了...XD

##3. 為什麼我build android的時候，`platforms/android/assets/www`底下的檔案常常沒有被更新？  
A: 之前曾經被這個問題搞到很頭大，連`cordova platform rm/add`之後都一樣，後來發現似乎是WebStorm的問題...。WebStorm的自動儲存機制好像是先寫到cache中，等累計一部份或是超過一定時間後再寫入disk的實體檔案裡，因此如果改完之後馬上進行build，會讀到尚未被寫入的資料。解決方法是變更後加個ctrl + s(save all)就可以了，不過上面那段原因純屬個人猜測，有錯的話煩請指正。

##4. 為什麼用camera拿到的圖片沒有EXIF?
A: `targetWidth`跟`targetHeight`屬性會把EXIF資訊消掉。

##5. 他媽的為什麼Samsung拍出來的圖片總是不會自動轉正，我明明有下`correctOrientation: true`了啊！？
A: 就叫你愛用國貨（誤。這是另一個讓我很頭大的問題，如果你用`saveToPhotoAlbum`儲存的話，會發生一些很奇妙的事：在相簿中瀏覽的縮圖是轉過的，可是點進去之後的圖片又是沒轉正的；目前是到的解法是自己把暫存copy一份出來存在另外的資料夾裡，這樣在手機相簿裡縮圖和內容點進去就都會是正的（不過app裡面的img還是要自己下css rotate轉）。當你拉到電腦上用一些可以截取EXIF的軟體看，會發現它其實還是沒轉過的，只是大多數的圖片顯示程式都會去讀EXIF裡面的Orientiation然後自動把它轉正，不過也有另外一種做法，例如HTC跟iphone都是直接給你轉正後的檔案。

##6. iOS傳檔案的時候拿到transferFileError Code:3?
A: 原因很多，通常code3會伴隨500 Error出現，這時候就要問server端到底發生什麼事了。我遇到的狀況是使用預設的fileTransferOption會傳出不帶副檔名的檔案，如果server端是從檔案名稱截取副檔名來判斷類型的話就會出錯，只要不要偷懶自己設定fileTransferOption就OK了。

##7. 在Android 4.4，從"最近使用過的檔案"之類4.4新增的資料夾選取的檔案，上傳時fileTransfer會拿到null?
A: 原因同Q2，plugin不認識這些4.4新增的路徑，靜待fileTransfer plugin更新，或是跟Q2那篇一樣自己在java檔加入kitkat的hack。