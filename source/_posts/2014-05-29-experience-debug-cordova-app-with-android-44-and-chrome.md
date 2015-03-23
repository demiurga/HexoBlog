---
layout: post
title: '[心得] Debug Cordova app with Android 4.4 and Chrome'
date: 2014-05-29 03:43
comments: true
categories: 
---
在Android 4.4，有新的方式可以用Chrome debug Android 上面的 WebView
方法超簡單（以下以cordova app當範例）：
1. 確定你有Chrome 30+, Android 4.4, 還有一個嗷嗷待哺的cordova app
2. 用USB把他們連起來，選項該的開該打勾的打勾，就跟平常一樣
3. 在你的cordova app的`onDeviceReady`事件加上這段
        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            if ( 0 != ( getApplcationInfo().flags &= ApplicationInfo.FLAG_DEBUGGABLE ) ) {
                WebView.setWebContentsDebuggingEnabled(true);
            }
        }
4. 切到chrome的`chrome://inspect/#devices`頁面
5. 勾選Discover USB Devices，應該會看到你的安拙在上面，接著開啟你的cordova app
6. 現在應該會列出你的app開啟的WebView(通常只有一個)，點下inspect，熟悉的ChromeDevTool就出現了！
    
感覺就跟Safari的web inspecter一樣
 
