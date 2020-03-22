---
title: '想知道嗎：var,let跟const的三角關係'
date: 2020-03-22 17:53:39
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/var-let-%E8%B7%9F-const-%E7%9A%84%E4%B8%89%E8%A7%92%E9%97%9C%E4%BF%82-f29010dc4f7e)

除了在學習 JS 之初有看過相關的資料，之後並沒有徹底釐清之間的差異，在使用了一段時間之後再次回顧，決定用一篇文章來好好整理一下。所以…開始吧！

比較有疑惑的應該就是 `var` 跟 `let` 了吧？

首先要先知道一件事，`var` 是在 ES6 之前的產物，在ES6之前，JS 的世界觀並沒有[區塊域(block)](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/block)的概念，因此會用 `var` 宣告所有的變數。ES6 之後殺出了一個強力小三 let ，她的出現大大的威脅了 `var` 的地位，讓我們先來看看可憐的 `var` 有什麼想說的。

[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/var) 裡面這樣說道：

> 以 `var` 宣告的變數, 其作用範圍 (scope) 及於該函數之內; 但是如果在函數外宣告, 其作用範圍則為全域性 (global)

簡單來說就是在函數(function)內宣告的 `var` ，在函數之外會讀不到他的值，但是在函數之外宣告的 `var` ，函數內會知道他是誰，太多字了嗎？看這個簡單的例子就懂了：

```
function fun(){
    var apple = 2;
};
console.log(apple); //會回報 "apple is not defined"

// 但是如果是這樣
var banana = 3;

function fun2(){
    console.log(banana);
};

fun2(); //會順利回報 3 出來
```

似乎很簡單的規則啊…但是這樣有一點小缺點，也就是會有 ***『區域變數覆蓋全域變數』*** 跟 ***『循環變數洩漏為全域變數』*** 的狀況，這兩個東西又是什麼呢？可以看下面的例子：

```
//區域變數覆蓋全域變數
var banana = 3;

function fun2(){
    console.log(banana);
    var banana = 4;
};
fun2(); // 喔不！ banana 變成 undefined 了！
```

另外一個是：

```
//循環變數洩漏為全域變數
var cake = ['q','w','e','r'];

for (var i=0; i<cake.length; i++){
    console.log(cake[i]);
};

console.log(i);
//會回報 i = 4
//喔不！“i” 怎麼跑出來了！我只想要在 for 迴圈的時候用它而已啊！
```

所以為了迴避這樣的情況，`let` 就此誕生！[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/let) 裡面說道：

> `let` 可以宣告只能在目前區塊、階段或表達式中作用的變數。

所以她比較乖，她不會隨便蓋掉別人的東西，也不會亂跑出去我不希望她去的地方，是以當前作用的區塊為界的變數，近乎是 `var` 的強化版。

那我很常使用的 `const` 又是什麼呢？

[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/const) 裡面這樣介紹他：

> `Constants` (常數)具有區塊可視範圍。常數不能重複指定值，也不能重複宣告。

他跟 `let` 一樣是以作用的區塊為界，但是他在最一開始宣告的時候就要賦值，而且不能重複賦值，是只能宣告一次的變數。

因此，建議在開發的專案中，盡可能地使用 `const` 跟 `let` 而不是 `var` ，避免一不小心讓 `var` 出來的變數跑來跑去，更能確保專案的穩定性唷！