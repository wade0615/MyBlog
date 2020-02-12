---
title: 想知道嗎：JS-Selectors
date: 2020-02-12 10:38:53
tags:
category: 想知道嗎
---

[Medium](https://medium.com/@wsw0615/%E6%83%B3%E7%9F%A5%E9%81%93%E5%97%8E-js-selectors-3c8e3f4de93a)

自己在參與一些簡單的活動與有趣的專案時，提前接觸到了 JS 相關的應用，查閱資料的時候發現，要用 JS 選取到我想要的 HTML DOM 元素時，有眼花撩亂的四種方式！我的天…搞什麼啊…因此決定簡單的記錄一下這件事。

## [document.querySelector](https://developer.mozilla.org/zh-TW/docs/Web/API/Document/querySelector)
* 回傳 document 第一個符合特定選擇器群組的元素
* 只有第一個！
* 可以藉由語法 `.class` 跟 `#id` 選到 class 或 id

## [Document.querySelectorAll](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelectorAll)
* 返回與指定的選擇器組匹配的文檔中的元素列表。返回的對像是 NodeList 。
* [NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList) 並不是 Array, 但是被允許可以用 `forEach()` 的方式來進行操作。 他也可以藉由 `Array.from()` 的方式來轉換成一個真正的陣列。

## [document.getElementById](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById)
* 回傳一個符合 id 選項的元素
* `getElementById()` 只有在作為 document 的方法時才能起作用，而在DOM中的其他元素下無法生效。

## [Document.getElementsByClassName()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByClassName)
* 注意我是 Elements ，有 s ，`getElementById` 沒有 s
* 針對所有給定的 class 子元素，回傳類似陣列的物件。（似乎不是NodeList唷！

常見的這四種方式中可以發現，`querySelector` 其實已經可以做到 `getElementById` 跟 `getElementsByClassName` 在做的事了，我的疑問是…那既然 `querySelector` 這麼厲害，為何還需要其他這兩個東西呢？有了 `querySelector` 之後，~~要你們何用？（誤~~

目前我只有找到一個小線索，也就是 `getElementById` 跟 `getElementsByClassName` 在 1998 年的 [Spec — Document Object Model (HTML) Level 1](https://www.w3.org/TR/REC-DOM-Level-1/level-one-html) 就有出現了，然而 `document.querySelector` 在 2013 年的 [Spec — Selectors API Level 1](https://www.w3.org/TR/selectors-api/) 才被我找到，似乎是 `document.querySelector` 比較晚出現啊～或許是因為…雖然後來出現了方便且功能強大的 `document.querySelector` 可以做到兩位前輩在做的事，但是為了比較好的維護在他出來之前就使用 `getElementById` 跟 `getElementsByClassName` 的程式碼，所以就沒有淘汰他們了吧（？

然而這個結論也都只是我的猜測而已，並沒有找到一個實質的證據來支持並證實我的猜測，如果有路過的大神知道為什麼的話～拜託留言或是寄信告訴我吧～我是真的很好奇想知道啊啊啊！
