---
layout: post
title: '[筆記] Cordova with jQuery Mobile & Backbone的一些小問題'
date: 2014-04-30 06:05
comments: true
categories: 
---
小弟這幾個月做前端的心得是：有些東西看起來很美好，睡到床上後才知道...（喂
下面筆記一下使用jQuery Mobile 1.4.2 + Backbone + Cordova 3.4開發中遇到的一些“小”問題

##Cordova
---
####Q: 為什麼我明明照[官方Doc](http://cordova.apache.org/docs/en/3.4.0/config_ref_images.md.html#Icons%20and%20Splash%20Screens)的說明放好icons了，build出來的app圖示還是那隻小機器人？
A: 因為愛，愛一直都是沒有理由的...好吧，老實說我不知道這是新版的cordova bug還是有什麼地方出了問題，導致cordova add platform的時候生出來的icon永遠都是那隻小機器人。解法有二：一是自己在不同平台中替換，ios的位置放在`platforms\ios\{projectName}\Resoures\icons`裏面，也可以直接用xcode換（其實這樣比較快，省得取一堆檔名），在Basic settings裡面就找的到，應該不用截圖了XD。android的位置則是在`platforms\android\res\drawable-{dpi}`裡面，Eclipse裡面應該也有地方可以換，不過公司電腦開Eclips超慢所以我沒開過...。第二種方法是用別人寫好的hook幫你做上面那些事，例如[這個](https://gist.github.com/apla/6179863)。
不過，用上面第一種方法做替換的話，cordova重新建平台的時候又會洗回來一次(rm/add platform時，build不會)，所以並不是個很美好的解法，不過當你被死線追著跑的時候就先將就頂著吧...Orz

####Q: 為什麼config.xml上的Orientation明明有設定portrait了，可是android上卻沒有用？
A: 根據stackoverflow上[這篇回答](http://stackoverflow.com/questions/21212246/cordova-ignores-screen-orientation-lock)的說法，這是個bug，不過他應該已經被fix掉了才對，那為什麼還會有問題呢？去檢查AndroidManifest.xml中的設定後，發現`android:screenOrientation`是`userPortrait`，兩者間的差別在於：
>If the user has locked sensor-based rotation, this behaves the same as portrait, otherwise it behaves the same as sensorPortrait.

可是不對啊，我明明是下portrait，為什麼會變成userPortait？打開cordova裡面一隻叫做`android_parser.js`的檔案，會發現下面這段：
```js
// Set the orientation in the AndroidManifest
var orientationPref = this.findOrientationPreference(config);
if (orientationPref) {
	var act = manifest.getroot().find('./application/activity');
	switch (orientationPref) {
		case 'default':
			delete act.attrib["android:screenOrientation"];
			break;
		case 'portrait':
			act.attrib["android:screenOrientation"] = 'userPortrait';
			break;
		case 'landscape':
			act.attrib["android:screenOrientation"] = 'userLandscape';
		}
}
```
簡單來說，就是cordova在build android時會把"portrait"轉成"userPortait"，"landspace"轉成"userLandspace"，不知道該說她貼心還是雞婆...
解法就是把上面的code稍微改一下：
```js
// Set the orientation in the AndroidManifest
var orientationPref = this.findOrientationPreference(config);
if (orientationPref) {
	var act = manifest.getroot().find('./application/activity');
	switch (orientationPref) {
		case 'default':
			delete act.attrib["android:screenOrientation"];
			break;
		case 'portrait':
			// act.attrib["android:screenOrientation"] = 'userPortrait';
			act.attrib["android:screenOrientation"] = 'portrait';
			break;
		case 'landscape':
			// act.attrib["android:screenOrientation"] = 'userLandscape';
			act.attrib["android:screenOrientation"] = 'landscape';
		}
}
```
####Q: 在config.xml中設置中文的name會不會有問題？
A: 老實說，這問題很微妙，因為我在config.xml中設定app name為中文時，build android會發生問題，可是華文圈好像沒有相關災情傳出...如果跟我一樣碰到的話，就在發佈前去`platforms/android/res/values/string.xml`裡面改吧。


##jQuery Mobile
---
####Q: changepage的時候會抖一下，為什麼？
A: 就跟你上廁所的時候會抖一下是一樣的道理（誤
這是很多人都會碰到的問題，不知道為什麼在transform的時候page的高會多出1px，結束後套上正常的min-height就產生了那一點點的抖動。解法有很多種，[這個](http://outof.me/fixing-flickers-jumps-of-jquery-mobile-transitions-in-phonegap-apps/)試過可以，不過會產生另一個問題就是ios會不能捲動page content，需要[iscroll.js](https://github.com/cubiq/iscroll)；後來發現johnbender[有解釋這個問題](https://github.com/jquery/jquery-mobile/issues/2846)，原因是jQuery mobile會預先向下scroll 1px來隱藏nav bar，解決方法就是在jQuery mobile引入後，還沒初始化頁面前設定$.mobile.defaultHomeScroll=0。
> 寶傑，你知道嗎，這1px搞了我兩天...

####Q: changepage的時候會閃一下，為什麼？
A: 首先，先確認不是你的眼睛在閃（無誤），畢竟幹這行可說是靠眼睛吃飯的，要好好保養啊...
如果眼睛沒問題的話，那就在page style上加個`backface-visibility: hidden`就可以了。

##Backbone
---
####Q: touch event會被觸發兩次，或是某些頁面的even時好時壞？
A: 檢查有沒有發生"ghost view"的狀況，也就是某個（或某些）dom被綁了一個以上的view，這種事情常發生在動態新增view的時候。
