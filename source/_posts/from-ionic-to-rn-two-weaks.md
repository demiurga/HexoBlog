title: "React Native 兩週心得"
date: 2015-06-05 23:26:01
tags:
---


最近在port一個大概半年的Ionic專案到React-Native上，分享一下到目前為止的心得。

首先，很想吐。不是我懷孕了，是doc實在是太簡略了...拿[LayoutAnimation](https://facebook.github.io/react-native/docs/layoutanimation.html#content)來說好了，在我寫這篇文章的時候React-Native已經發布了兩個月，而LayoutAnimation的頁面長這樣：

```
LayoutAnimation
Methods
static configureNext(config: Config, onAnimationDidEnd?: Function, onError?: Function)
static create(duration: number, type, creationProp)
```
最好有人能猜出來Config長怎樣啦（翻桌）

類似的狀況在很多地方都有發生，而且常常是越重要的部分越省略。這種狀況下javascript軟型態的缺點就展露無遺，不像C#或JAVA之類的強型態語言，點進去看class總會有個答案。更糟糕的是，在React-Native，你必須一路trace進Objective-C才會知道這東西的原理...現在可以理解我想吐的原因了吧。

再來，是Style的問題。個人認為 *CSS的subset* 這個說法是個強烈的誤導，事實上這只是 *很像CSS* 而已；比方說沒有CSS中最重要的根本屬性`display`、`width`跟`height`只接受`px`而不接受`%`、圖片多了個`resizeMode`屬性(而且又沒有列出來...)、某些Component不支援某些屬性...等等，夭壽多的問題都是doc上完全沒有提到的。

接下來是Debug tool的問題。這方面同樣的不是很友善，因為目前版本(0.4.4)的packager不支援第三方的sourcemap，導致你果寫ES6(with Babel) / TypeScript / CoffeeScript 的話，就必須自己從他報錯的`.js`行數找到`.es6``.coffee``.ts`裡相對應的地方才行，而在chrome裡下斷點的時候也只能在`.js`裡操作。聽說`0.5 stable`會改用Babel當packager（不過只開放部分功能），應該可以解決這個問題。

雖然剛剛抱怨了一堆，但我還是看好這個東西，畢竟他從根本上解決了Hybrid App一直以來的效能問題，而且有一個夠大的社群、一個成功的範例（Facebook Group on iOS）和一個強大的老爸（Facebook這幾年在前端應該算是毫無疑問的領頭羊）。不過我真正期待的是聽說9月會出現的`Android`版，畢竟Hybrid App Programers最頭痛的就是Android上的效能（連Crosswalk這種東西都逼出來了），iOS目前反而比較像錦上添花的感覺；而且以系統架構來看，Android版的實現絕對比iOS要困難許多。揪竟～React-Native for Android會給我們驚喜還是失望呢？敲碗期待中
