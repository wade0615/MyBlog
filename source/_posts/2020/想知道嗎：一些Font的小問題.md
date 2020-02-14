---
title: 想知道嗎：一些Font的小問題
date: 2020-01-28 20:12:00
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E6%83%B3%E7%9F%A5%E9%81%93%E5%97%8E-%E4%B8%80%E4%BA%9Bfont%E7%9A%84%E5%B0%8F%E5%95%8F%E9%A1%8C-1ea60065b372)
來紀錄一點點在使用font 上遇到的一點小狀況吧！

## 先來簡單的複習一下 px ex em rem 分別是啥

* px：是一個相對單位，相對於「顯示設備 pixel 的高度/寬度」。
* ex：定義「x-height 的高度」也就是說它是一種高度單位，是字的最小方形基準尺寸。是當下font-size的小寫x高的大小。
* em：定義「M 字母寬」也就是說，它是一種寬度單位，是字的最大方形基準尺寸。當下font-size的大寫M寬的大小。
* rem：瀏覽器設定字型的 em，等同於在 body 的 em。

## font-awesome 小問題

* font-awesome 是被設計用來緊貼使用者專案中前後 HTML 文檔的 inline 元素
* 而且會繼承其父層的 CSS 字體設定 style
* 所以他是『字』！他是『字』！
![img](https://miro.medium.com/max/546/1*6kgxWGNk0iS8CtVzcjmTVw.png =50%x)![img](https://miro.medium.com/max/545/1*xo-hIhbbpn1w-lV2bfGHKg.png =50%x)

要記得把他當成字來用喔！

## google瀏覽器 min-font 限制

* chrome 瀏覽器會將字型小於 12px 的字形限制為 12px
認為低於12px的字對使用者閱讀是不友善的
* 所以只要使用 12px 以下的字形都會呈現 12px 的樣式
但是 line-height 會跟著改變喔！
* webkit-transform: scale(0.??) 目前暫時可以將字變得更小
然而看來 transform 會破壞字的間距，使用上要再小心跟注意喔！
這篇拖稿拖了快三個禮拜，趁年假期間趕快補上囉～