title: 2015上半年回顧
tags:
---

不知不覺半年了啊... 這半年真的發生了不少事，各方面來說都是

## Front End

#### Ionic

從當初還在nightly就開始拿來做專案，寫個人作品，到現在也一年多了，雖然不敢自稱大師，但基本上已經玩到沒有什麼好玩了。反正Angular就那樣，上再多奇技淫巧也就是那樣...或許Ionic也這樣覺得吧，所以終於甘心上1.0.0了(應該是為了專心玩Ionic2)。個人一直很喜歡Ionic這個team，他們有些性格感覺跟我很像，像是超級龜毛、講究質感、不喜歡給時限承諾(正好都是一些台灣老闆不需要的東西XD)，雖然React-Native出了之後Cordova聲勢低落，不過Angular2在上次Conf的Demo相當驚人，讓人很期待Ionic2會做出怎樣的東西。

#### React

因為在Mobile上被Angular折磨太久傷得太深，所以跑去玩看起來很潮的React。寫了幾個Project之後漸漸地有了一點feel，html-css-js三位一體的想法感覺很有趣，而且社群很大，親爸爸Facebook養了兩億支的白老鼠每天幫她做測試(XD)。不過缺點就是樣板的味道很重，要多寫很多code，尤其是想做一些比較風騷的操作時就會很明顯的感受到不悅。而且在jsx塞props的時候很難有效的得到code intelligence的支援（至少WebStorm跟Atom都做得不好），這對大專案可能會有點麻煩，或許Facebook自家的Nuclide會做一點優化？

#### Flux



#### Flow

感覺很不錯，可以得到強型態的支援，又不會像typescript那樣死板板需要`.d.ts`；可是一來WebStorm沒有支援，二來...[所以我說那個Windows呢？](https://github.com/facebook/flow/issues/6)

#### WebPack

我只是想寫個摳，為什麼要這麼麻煩...那群大師跟之父為什麼沒有早點把Module寫在語言規範裡，這幾年前端的重複造輪子大賽一點都不有趣阿Orz。




## /.+/Script




## Mobile

#### Cordova

已經熟到爆炸的東西，因為各種討厭的理由寫了許多Native Plugin（看wiki學Obj-c XDD），也改過原本的FileSystem，簡直快變成Mobile工程師了...。對於一個處女座A型的前端工程師而言，Cordova@Android那糟糕的render效能實在是令人十分痛苦（加Crosswalk也沒好到哪去），還有那討厭的生命週期。如果React Native或Native Script有發展起來的話，之後應該不會在碰這個了...

#### React Native

目前正在利用空檔時間把舊的Cordova產品搬上去，分享一下到目前為止的心得：
* Document嚴重需要補強，很多東西都藏在source code裡面，沒去trace會漏掉很多feature
* 有Bug是很正常的，不過很難trace就不事件好事了...目前packager沒有支援第三方的sourceMap(ex: Babel, typescript)
* team好像快掛了，issues跟PR都嚴重山積XD

個人對這個Project的期望還是很高，畢竟頂著Facebook&React的光環，希望他能成為Hybrid App開發者的最終解答。

#### Native Script

Telerik家的產品，比React-Native早很多推出，不過社群一直不夠大。試玩的感想是layout方面很難做細部的客製化，不過架構上我覺得比React-Native更簡單強壯，不過沒有紅起來還是令人怕怕的，看Angular2正式發布之後會不會帶來一波新人潮吧。

#### WinRT

玩了個todo就沒接下去玩了。js身為第一級語言的感覺很爽，可是這市場份額實在讓人提不起勁...等Win10看看會有什麼改變吧。

#### Dart with Sky framework

Dart想在browser上取代javascript的計畫失敗了，這次呢？從發表會寒酸的樣子來看，感覺不出Google有在Dart@Android上面下真功夫的企圖，反而有種很重的倉促感。不過畢竟這還只是非常早期的Project，就靜觀其變吧。


## IDE

#### WebStorm

去年年底買了WebStorm的Licence，用了半年之後有點想跳槽的感覺...。先說優點吧，js的code intelligence很強，目前我覺得準確度只略輸Visual Studio。再來就是前端熱門的工具幾乎都有內建的輔助支援，設定欄位點幾下就可以用了，在現今前端各種混亂的情況下這爽度真的很高，可以讓你少幹很多無謂的事（我只是想寫個摳阿老大）。再來說說缺點，首先是效能...真的很慢，真的﹑真的﹑很慢。以我家的E5 + 16G Ram來說，寫React時`componentDidMount`這行字打完有九成的機率代碼提示不會出現（剩下的一成是突然忘記怎麼拚）。公司的Mac就更慘了，因為我工作需要同時開WebStorm*2 + AppCode + Intellij　+ Xcode，有時候真的會很想殺人。再來是支援的問題，畢竟是一年才改版一次的付費軟體，支援性終究比不上Atom﹑ Sublime來的快，有些比較小眾的東西就沒辦法用，例如jsx-typescript、cjsx(coffee-jsx)、WebPack以及Flow（這沒支援令我蠻訝異的），然後你也只能雙手合十祈禱DevTeam有聽到你的聲音，這是我想跳槽的主因。

#### Atom



#### Nuclide

只差20個就有封測了阿阿阿阿阿（抱頭）。希望能早點Release，不然寫個React-Native要開兩三個IDE實在很煩...
