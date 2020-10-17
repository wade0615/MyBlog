---
title: 想知道嗎：JSforEach的index位置
date: 2020-08-27 11:14:03
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E6%83%B3%E7%9F%A5%E9%81%93%E5%97%8E-js-foreach-%E7%9A%84-index-%E4%BD%8D%E7%BD%AE-37f8751c73a2)

![img](/images/2020/想知道嗎：JSforEach的index位置/JS.jpeg =100%x)

前一陣子在應用 JS 的 [Array.prototype.forEach()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) 時，因為有需求要調用 index 這個隱含參數，然而在 coding 的過程中稍不留意，在不同的地方寫出了類似以下兩種將 e 跟 index 位置互換的寫法

```
var array = [1,2,3]
array.forEach((e,index) => console.log((e + 1),index))
array.forEach((index,e) => console.log((e + 1),index))
```

一時之間還沒注意到有什麼不同，然而到我自己在 code review 檢查時，除了發現自己怎麼有兩種寫法外，還發現了這樣會使 console.log print 出來的東西略有不同

<img src="/images/2020/想知道嗎：JSforEach的index位置/code1.png" style="display: block;margin: 0 auto;width: 60%">
<img src="/images/2020/想知道嗎：JSforEach的index位置/waitWhat.gif" style="display: block;margin: 0 auto;width: 50%">

我以為的 e 應該就是 陣列目前正在 forEach 處理的單項元素，而 index 就是 陣列目前正在 forEach 處理的元素索引 ，其實這樣的理解也沒有錯，但我以為的是，第二種寫法：

```
array.forEach((index,e) => console.log((e + 1),index))
```

也會 print 出這樣的結果：

<img src="/images/2020/想知道嗎：JSforEach的index位置/code2.png" style="display: block;margin: 0 auto;width: 7%">

然而結果居然與我想的不一樣！各位讀者知道為什麼嗎？
以下要來公布解答囉！

---

百思不得其解的情況下，求助了當初在 好想工作室 同期的學員，我們的班長 Yachen大人，而她一語道破我的盲點...

> Yachen：foreach(item, index) 是 js 的隱含參數，第一個參數就是遍歷的元素，第二個參數是 index，即使改參數名字，第一個也一定是遍歷的元素，第二個一定是 index，名字只是障眼法哈哈

完全點破啊！看完立刻了解，我只是將 陣列目前正在 forEach 處理的單項元素 命名從 e 改成 index ，而將 陣列目前正在 forEach 處理的元素索引 命名從 index 改成 e ，他的本質意義並沒有改變，畢竟 callback 函數中所帶的參數，並不是用名字來區分，而是依次向callback函數傳入三個參數啊！

<img src="/images/2020/想知道嗎：JSforEach的index位置/ohNo.gif" style="display: block;margin: 0 auto;width: 40%">