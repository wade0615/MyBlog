---
title: 想知道嗎：What the hack is the Event-loop in JS?
date: 2021-06-27 14:36:43
tags:
category: 想知道嗎
---


![與本文無關但我就是想放的，綾波零ByQosicart](/images/2021/想知道嗎：WhatTheHackIsTheEvent-loopInJS/Eva0.jpeg =100%x)

#### 本篇文主要整理 Philip Roberts 在 JS Conf 的演講影片 [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=526s&ab_channel=JSConf)

好的，轉職成前端工程師後這段時間，我聽過一些像是『JS單執行緒』『Call back』『Event loop』『V8 引擎』『Runtime』等等的名詞，但我從來沒有真正了解其中的關聯與意義，所以…JavaScript，這些名詞對你來說倒底是什麼意思？

![JS](/images/2020/想知道嗎：JSforEach的index位置/JS.jpeg =100%x)

## 單執行緒（one thread）＆堆疊（Stack）

one thread == one call back == one thing at the time，意思就是 JS 一次就只會做一件事。

而 Stack 則是會記錄我們目前程式執行到哪裡，將目前執行的程式添加到『堆疊（Stack）』的最上方，直到執行完之後再將他從『堆疊（Stack）』的最上方移除，並遵循「後進先出」的原則執行堆疊中的下一段函式，一次只做一件事。

像是 [這樣](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=270s&ab_channel=JSConf)

![stack](/images/2021/想知道嗎：WhatTheHackIsTheEvent-loopInJS/stack.png =100%x)

那如果你的程式出現了無限迴圈，函式就會不斷地被添加到 Stack 上面，最後被瀏覽器阻止…

像是 [這樣](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=405s&ab_channel=JSConf)

![toManyStack](/images/2021/想知道嗎：WhatTheHackIsTheEvent-loopInJS/forEachStack.png =100%x)

---

## 阻塞（blocking）

Blocking 其實沒有一種明確的定義，如果有一段程式碼跑得太久了，卡在 Stack 上面一直結束不了，造成網頁不管怎麼點都沒有作用沒有反應，就被稱作 Blocking。

像是 [這樣](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=472s&ab_channel=JSConf)

![blocking](/images/2021/想知道嗎：WhatTheHackIsTheEvent-loopInJS/blocking.png =100%x)

---

## Wait…What???

那該怎麼辦？我們總有一些函式是需要等待之後才能有結果的，總不可能就讓畫面停在那邊等待吧？這時我們會很直覺地想到使用 非同步（Asynchronous Callback） 的方式去撰寫程式。

試想一下下面這種情境：

![setTimeout](/images/2021/想知道嗎：WhatTheHackIsTheEvent-loopInJS/setTimeout.png =100%x)

我們可以看到 JS 依序做了幾件事：

1. 印出 hi
1. 我們叫它等待五秒之後印出 there
1. 然後 JS 不知怎麼的一邊數著五秒但一邊的跳過了 “等待” 的動作
1. 印出 JSConfEU
1. 五秒到了，印出 there

好像哪裡怪怪的…如果 JS 一次只能做一件事，那他是怎麼同時 “數著五秒” 又同時 “印出 JSConfEU” 的呢？依據剛剛的說法他不是會使網頁 Blocking 並 “數完五秒” 再做其他事嗎？

![what](/images/2021/想知道嗎：WhatTheHackIsTheEvent-loopInJS/what.gif =50%x)

---

## Concurrency and Event Loop

事實是，Javascript 的運行環境 (Runtime) 的確一次只能做一件事情，我們可以同時做很多事情，是因為瀏覽器不是只是一個運行環境 (Runtime) 而已，瀏覽器提供了我們很多不同的 [WebAPIs](https://developer.mozilla.org/en-US/docs/Web/API) ，就像這次我們用的 [setTimeout()](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) ，這些不是 JS 引擎本身的功能也不在 V8 引擎中，我們只能呼叫這些功能去執行他，然後搭配 Event Loop 在需要這些函式的時候，加入我們的 Stack 中去執行它。

就像 [這樣](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=726s&ab_channel=JSConf)

![eventLoop](/images/2021/想知道嗎：WhatTheHackIsTheEvent-loopInJS/eventLoop.png =100%x)

所以回想一下上面的 setTimeout() 情境，正確的執行順序應該是：

1. 印出 hi
1. setTimeout() 的等待事件被 WebAPIs 取走執行等待五秒，而 Stack 繼續下一步
1. 印出 JSConfEU
1. 五秒到了，WebAPIs 把要做的事...也就是 console.log('there') 放入 Event Queue 中
1. 當 Stack 中沒有其他任務了，Event Loop 開始做事，將 Queue 中的任務依序推去 Stack 中執行
1. 印出 there

就像 [這樣](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=807s&ab_channel=JSConf)

![queue](/images/2021/想知道嗎：WhatTheHackIsTheEvent-loopInJS/queue.png =100%x)

---

## setTimeout() 時間為 0 呢？

由於上面 WebAPIs 與 Event Loop 的存在，我們知道只要你使用了像是 setTimeout() 這種 WebAPIs ，這項任務就會被移出 Stack 並交由 WebAPIs 去執行 runtime ，結束後放到 Queue 去等待 Stack 被清空。

結果依然會是：

1. 印出 hi
1. setTimeout() 的等待事件被 WebAPIs 取走執行等待 0 秒，被立刻放入了 Event Queue 中等 Stack 清空
1. 印出 JSConfEU
1. 當 Stack 中沒有其他任務了，Event Loop 開始做事，將 Queue 中的任務依序推去 Stack 中執行
1. 印出 there

所以重點不是 "時間為0" ，而是我們用了 setTimeout() 這種 WebAPIs 。

就像 [這樣](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=899s&ab_channel=JSConf)

---

本文主要擷取 [所以說event loop到底是什麼玩意兒？| Philip Roberts | JSConf EU](https://www.youtube.com/watch?v=8aGhZQkoFbQ&ab_channel=JSConf) 的部分內容作為筆記，並參考了網上許多前輩的整理文章與 MDN 內容加以閱讀理解，有 30 分鐘的非常建議看完 Philip Roberts 完整的影片，真的幫助了我許多。

影片講者 Philip Roberts 也製作了一個視覺化的工具 [Loupe](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D) ，幫助理解整個事件過程，可以自己將想觀察的案例貼上去自己觀察。

* [並行模型和事件循環](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/EventLoop)
* [[JavaScript] Javascript 的事件循環 (Event Loop)、事件佇列 (Event Queue)、事件堆疊 (Call Stack)：排隊](https://medium.com/itsems-frontend/javascript-event-loop-event-queue-call-stack-74a02fed5625)
* [[筆記] 理解 JavaScript 中的事件循環、堆疊、佇列和併發模式（Learn event loop, stack, queue, and concurrency mode of JavaScript in depth）](https://pjchender.blogspot.com/2017/08/javascript-learn-event-loop-stack-queue.html)
