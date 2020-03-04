---
title: 想知道嗎：Vertical-align狀況題
date: 2020-02-29 16:49:46
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E6%83%B3%E7%9F%A5%E9%81%93%E5%97%8E-vertical-align-%E7%8B%80%E6%B3%81%E9%A1%8C-c8882d6a4fba)

一週前其他學員們遇到了一個有關 vertical-align 的相關情境題，一開始是兩個人在討論，後來一個問一個，最後變成一大堆人圍觀並且辯論，然而都無法給出一個很好讓大家理解的說法，好奇心的驅使之下，使路過的我也加入了討論，那問題是什麼呢？

下圖為這次的狀況題，紅線為借用綠方塊的偽元素製作出來象徵 “I’m an IFC”文字與綠方塊的 baseline。

![](/images/2020/想知道嗎：Vertical-align狀況題/1_bzBQbsHnp691nHbgH7YJwA.png)

問題很簡單，「為什麼上圖的 baseline(紅線) 會像是懸空的樣子呢？」

## 首先先來釐清兩個定義

* vertical-align 的意思是，用來指定行內元素（inline）或表格單元格（table-cell）元素的垂直對齊方式。不指定的初始值為baseline。參考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)
* 一個 line-box 的高度決定於位置最高的 inline-level box 頂部與位置最低的 inline-level box 底部的距離。參考[這篇鐵人](https://ithelp.ithome.com.tw/articles/10225029)

所以一個是決定『對齊方式』，一個是決定『空間高度』，兩件事沒有直接關係喔！

什麼意思呢？讓我們先簡化這件事，vertical-align 是指定元素的對齊方式，那 “I’m an IFC” 跟 “綠方塊” 的對齊方式是啥呢？

沒錯，是 baseline 。

所以如果我把藍色方塊拿掉會長什麼樣子呢？

![](/images/2020/想知道嗎：Vertical-align狀況題/02.png)

疑？好像沒什麼問題啊～應該都還能理解吧～

現在！

我手邊有一個藍色方塊（沒錯就是你知道的那個），我告訴他：「你跟其他元素的對齊方式是 “vertical-align: top;” 唷！」

“vertical-align: top;” 是什麼意思？我們看[規範](https://www.w3.org/TR/CSS2/visudet.html#propdef-vertical-align)

![](/images/2020/想知道嗎：Vertical-align狀況題/03.png)

『使元素及其後代元素的頂部與整行的 line box 頂部對齊。』，意思是要我們拿藍色方塊的頂部去跟整行 line box 的頂部對齊，藍色方塊的頂部肯定沒什麼問題，那 “整行 line box 的頂部” 在哪呢？

![](/images/2020/想知道嗎：Vertical-align狀況題/04.png)

就是在這裡啊！

因此我把藍色方塊擺進去就會像是這樣：

![](/images/2020/想知道嗎：Vertical-align狀況題/05.png)

注意此時綠方塊 & “I’m an IFC” 的 baseline 的位置依然是對齊綠方塊的底部與 “I’m an IFC” 的喔！

但是！我們有改變藍色方塊的 position 或是設定其他讓她脫離文檔流的方法嗎？並沒有！所以他還是在灰色框框內，藍色方塊還是屬於灰色框框子層的東西，這時候再看[規範](https://www.w3.org/TR/CSS21/visudet.html#line-height)：一個 line box 的高度是這麼決定的：

![](/images/2020/想知道嗎：Vertical-align狀況題/06.png)

『一個 line-box 的高度決定於位置最高的 inline-level box 頂部與位置最低的 inline-level box 底部的距離。』，還記得藍色方塊還是屬於灰色框框子層裡的東西嗎？這時候灰色框框裡面的東西的最低點在哪裡？

![](/images/2020/想知道嗎：Vertical-align狀況題/07.png)

在這裡啊！

所以灰色框框這個 line box 的高度就變成這樣囉：

![](/images/2020/想知道嗎：Vertical-align狀況題/08.png)

所以整個紅線以下的灰色區域都是因為 line box 的原則推出來的啊～跟 “I’m an IFC” & 綠色方塊沒有關係的唷！這樣有了解了嗎？