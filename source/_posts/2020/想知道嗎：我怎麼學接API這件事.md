---
title: 想知道嗎：我怎麼學接API這件事
date: 2020-04-08 00:57:28
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E6%83%B3%E7%9F%A5%E9%81%93%E5%97%8E-%E6%88%91%E6%80%8E%E9%BA%BC%E5%AD%B8%E6%8E%A5-api%E9%80%99%E4%BB%B6%E4%BA%8B-a46b10422bbf)

學習怎麼接 api 可以說是在學習 WEB 前端技術之路上一個很重要的里程碑，最一開始我是藉由 jQuery 的 `$.ajax() `來接 api 的，但在過程中遇到太多自己不懂的東西，又不懂「接api」這件事底層到底怎麼運行的情況下，決定在學習 [JS30](https://javascript30.com/) 的過程中，循著 [我每天都接一個api](https://ithelp.ithome.com.tw/users/20107688/ironman/1507) 的腳步，盡可能每天都接一個 api 來讓自己熟悉這件事。

來快速回顧一下我的學習之路吧！

---

## 要先知道

### API
* API全名叫做 「Application Programming Interface」
* 中文叫做「應用程式介面」
* 通常在寫網頁的時候，我們都會直接講 API，但其實我們指的是 Web API
* 程式跟程式之間的串接的一套標準

### Ajax
* 全名是「Asynchronous JavaScript and XML」
* 重點在於Asynchronous這個單字，「非同步」
* 『非同步的 建立一個XML(XHR)物件，打開網址然後送出要求，最後由回調函式處理伺服器傳回的Response(回應)』的一門技術。

參考 [AJAX與Fetch API](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/ajax_fetch.html)

## XMLHttpRequest

[MDN](https://developer.mozilla.org/zh-TW/docs/Web/API/XMLHttpRequest)

這可以說是我在 [我每天都接一個api](https://ithelp.ithome.com.tw/users/20107688/ironman/1507) 的過程中用到最多的一種方式，他可以說是 JS 最一開始接 api 的一種技術，個人滿喜歡胡立大大的這篇文章：[輕鬆理解 Ajax 與跨來源請求](https://blog.huli.tw/2017/08/27/ajax-and-cors/)，裡面詳細說明了許多會需要用到的技術，與最一開始用 WEB 接 api 會遇到的網頁問題。

```
// ㄧ個基礎的XMLHttpRequest物件如下
function makeARequest(){
  // 建立一個新 XHR Object
  var xhr = new XMLHttpRequest();
  
  // 用.open() method 開始一個 request
  // 必須傳入兩個參數，第一個是 HTTP method，第二個是 request 要傳向的目的地 url 
  // 也就是放你得到的 api url 的地方
  xhr.open("GET", "http://www.example.org/example.txt");
  
  // 當 request 成功完成之後，會執行的 callback
  xhr.onload() = function(){
   // Do something with the retrieved data
  };
  
  // 送出 request
  xhr.send();
  
}
```

## jQuery ajax

[jQuery.ajax()](https://api.jquery.com/jquery.ajax/)

這就是我最一開始在接 api 的方式，不得不說原生越學越久，其實也越能理解為何 jQuery 會曾經紅極一時，他真的簡化了很多步驟跟複雜的手續阿！但無奈如此方便的技術真的讓當初剛開始學習的我一頭霧水，也不能理解他幫助了我哪些事情，所以我也打算在我的學習之路上逐漸淘汰他了，但為了紀念我也還是把他放上來，畢竟這是我最一開始接 api 的方式。

```
var request = $.ajax({
  url: "http://www.example.org/example.txt",
  method: "POST",
  data: { id : menuId },
  dataType: "html"
})
    .done(function( msg ) {
    // Do something with the retrieved data
  });
```

---

## fetch

[MDN](https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch_API/Using_Fetch)

這大概就是在經歷以上歷程之後，現階段我如何接 api 的一種方式了，說時在其實我也還沒搞懂所有的語法跟技術，但就想先就我目前有用的部分做一點紀錄。

### fetch基本語法

[MDN](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch)

語法 `fetch(input,init)`

* input
    * url
* init
    * method
    * headers
    * body
    * mode
    * credentials
    * 等等

---

### method

定義 request 的類型：如 GET、POST。

### headers

```
{
Accept: "application/json; charset=utf-8", 
"Content-Type": "application/json; charset=utf-8"
}
```

* `application/json` 倒底是啥？他是一個 [MIME type](https://developer.mozilla.org/zh-TW/docs/Glossary/MIME_type)，也就是「内容類型」（content type），是一種用於標示文件類型、描述內容的隨附文件發送的標識字符串。
* `UTF-8`是一種針對 Unicode 的可變長度字元編碼。
* [Accept](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Headers/Accept) 告訴伺服器 用戶端可解讀的內容類型。
* [Content-Type](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) 告訴後端服務器實際發送的數據類型。

### body

[JSON.stringify](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 將一個 JavaScript 值轉換為一個 JSON 字符串

---

基本上這樣就可以成功發 api 出去了，那成功送出去之後，那時候與我配合的後端問了我一個問題：啊你有收到我的 response 嗎？
：蛤？

---

## fetch的回應

MDN 這樣寫 `fetch` 的返回值

![](https://miro.medium.com/max/365/0*3yAKdte1zF5NraNa.png)

一個 [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)？那是啥？Promise是一個物件，有三種狀態，等待中（pending）、完成（resolve or fulfilled）跟失敗（reject）

* 非同步操作的最終完成 fulfilled (或失敗 rejected ),及其結果值.
* 分為 resolve 和 reject 兩個參數

[Response](https://developer.mozilla.org/zh-CN/docs/Web/API/Response)

* 呈現了對一次請求的回應數據。

## .then

之後會看到我在裡面寫這樣：

```
.then(response => {
    result= Promise.resolve(response.json());
})
```

[Promise.prototype.then()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)

```
p.then(function(value) {
  // 當 Promise 被實現（fulfilled）時呼叫此函式。
});
```

[response.json()](https://developer.mozilla.org/zh-CN/docs/Web/API/Body/json)

* 返回一個被解析為 JSON 格式的 promise 對象

[Promise.resolve](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)

* 回傳一個以 value 判定結果的 Promise 物件。
* value？JS中的 Array、Object 等等…都是 value 啦~

```
.then(response => {
    result = Promise.resolve(response.json());
})
```

所以以上整段的意思就是：
我定義一個變數 result 來儲存

『我用 value 的方式來顯示 被非同步請求成功回傳 且被解析為 JSON 格式的結果』

### .then的回傳值

![](https://miro.medium.com/max/338/0*_-166zqei1jCWC7y.png)

要注意的是 `.then的回傳值` 是 [擱置狀態的Promise](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Promise#%E6%8F%8F%E8%BF%B0)，是一個可以被轉換為一個操作成功（fulfilled）的狀態或是操作失敗（rejected）的狀態。

要去注意 Promise 狀態（PromiseStatus），要是已經 操作成功（fulfilled）的狀態或是操作失敗（rejected）的狀態，才可以接續著做函式操作唷。

---

這大概就是我目前對「接 API」這件事全部的了解，對網頁的規範與限制還不夠了解，對 Promise 與非同步也還有可以進步的地方，我將會繼續成長繼續學習囉！

