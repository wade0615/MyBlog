---
title: 想知道嗎：Text-overflow把行尾變成…
date: 2020-02-19 16:52:03
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E6%83%B3%E7%9F%A5%E9%81%93%E5%97%8E-text-overflow%E6%8A%8A%E8%A1%8C%E5%B0%BE%E8%AE%8A%E6%88%90-e86ad8bf827c)

今天來講一點視覺呈現上的小東西，我們偶爾會在一些過長的段落結尾看到像以下這樣子的呈現

![textOverflow](/images/2020/想知道嗎：Text-overflow把行尾變成…/textOverflow.png)

也就是過長的段落文字被砍掉了，變成了一種未完待續的 “` … `”，這是怎麼做到的呢？

讓我們把目光轉到 CSS 樣式裡一個叫 `text-overflow` 的屬性，查閱 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-overflow) 可以看到

> text-overflow 屬性確定如何向用戶發出未顯示的溢出內容信號。它可以被剪切，顯示一個省略號（...）或顯示一個自定義字符串。

咦？好像是我們在找的效果欸～但是等等！好像還有些東西～再多讀一點

> 這個屬性只對那些在塊級元素溢出的內容有效，但是必須要與塊級元素內聯(inline)方向一致（舉個反例：內容在盒子的下方溢出。此時就不會生效）。文本可能在以下情況下溢出：當其因為某種原因而無法換行(例子：設置了”white-space:nowrap”)，或者一個單詞因為太長而不能合理地被安置(fit)。
> 這個屬性並不會強制“溢出”事件的發生，因此為了能讓”text-overflow”能夠生效，程序員們必須要在元素上添加幾個額外的屬性，比如”將overflow設置為hidden”。

喔喔喔！其實意思就是說必須要先有

`overflow : hidden;` 限制超出邊界的文字隱藏
`white-space : nowrap;` 限制文字不換行

然後
`text-overflow` 才會生效

稍微搞懂之後照著做應該都能成功啦～但是因為設定了`white-space : nowrap;` 的關係，整段文字只會有一行，所以只有單行文字的時候超出空間範圍才會變成`...`，那如果我想要做到兩行或是三行文字之後才變成`...`呢？該怎麼辦呢？

讓我們再看向另外一個東西 `-webkit-line-clamp` 的 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/-webkit-line-clamp) ，裡面提到

> -webkit-line-clamp CSS屬性可以把塊容器中的內容限制為指定的行數.
> 它只有在 display屬性設置成 -webkit-box或者 -webkit-inline-box 並且-webkit-box-orient 屬性設置成vertical時才有效果。

喔喔喔！看起來也很簡單嘛～但是

## 注意！注意！

瀏覽器支援度

![img](/images/2020/想知道嗎：Text-overflow把行尾變成…/compatibility.png)

也就是小廢物 ＩＥ 並不支援啊！而且看 [w3.org規範](https://drafts.csswg.org/css-overflow-3/#propdef--webkit-line-clamp) 可以知道這個屬性還在 Working Draft 階段，使用上要注意喔！