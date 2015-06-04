---
layout: post
title: '.NET NPOI'
date: 2014-01-01 15:08
comments: true
tags: [.NET]
---
每次要用都忘記用法，未老先衰阿...

筆記一些重點

1. usage: `NPOI`, `NPOI.HSSF.UserModel`
2. 先取得FileStream 
```
FileStream fs = new FileStream(url, FileMode.Open, FileAccess.Read);
```
3. 產生HSSFWorkBook 
```
HSSFWorkbook workbook = new HSSFWorkbook(fs)
```
4. 結構：Workbook -> Sheets -> Rows -> Cell
5. Cell不要直接`ToString()`，會拿到算式。有`NumericCellValue`之類的屬性可用
6. NuGet拿到的是1.5版，XSSF是2.0版的東西，不過還在alpha，有很多問題。
7. 要注意很多政府機關文件的格式會錯誤，要開啟後再另存。

小抱怨一下，某些政府機關的公開資料真的天殺的難找，還要從檔名逆推出文件格式編號再去猜相對應的路經...這是在玩國家寶藏嗎？