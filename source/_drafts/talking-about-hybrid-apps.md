title: 聊一聊Hybrid Apps這檔事
tags:
---

從時薪八塊半畢業以來，幾乎都在做Hybrid App，包括Smart TV, Cordova, WinRT with WinJS, React-Native,  還有自己玩的nw.js(aka: Node-Webkit)，雖然不敢自稱大師，但也有不少有的沒的心得感想與抱怨，下面就拿出來分享一下

###Why Hybrid Apps?
先聊聊為什麼要用Hybrid Apps好了，我想大部分老闆的想法都一樣：省錢。拿Cordova來舉例好了，
你找一個寫web的跟找一個寫Android的加一個寫iOS的再加一個寫WindowsPhone的(恩...好吧，可能不太需要)，這價錢起碼差了三四倍
：如果你找的web programer剛好有點認真有點強，還可以去凹他寫Native Plugin順便搞公司的網頁，反正web很簡單很快就搞定了嘛～

結果就是，一堆半生不熟的新手們，搞出一堆品質不佳的Hybrid App，然後不明究裡的局外人就開始說：啊～你看，javascript只是個toy language而已啦，慢得要死毛病又一堆，巴拉巴拉扒拉...

...話題好像有點跑遠了，讓我們回到Hybrid Apps上面。

###真的有比較省嗎？
如果你的老闆跟我之前呆過的某間公司一樣，本著做一個(專案)跑一個(客戶)的精神在做，那麼這的確很省...
不過如果你想build的是一個高水準、給數十萬或數百萬user良好體驗的app，那麼就不一定了。
