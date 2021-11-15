---
title: 想知道嗎：JS 中的 Promise & Async/Await 淺談
date: 2021-11-15 14:30:40
tags:
category: 想知道嗎
---

![img](/images/2020/想知道嗎：JSforEach的index位置/JS.jpeg =100%x)

> 這大概是我最想殺了自己的其中一次案例，由於 `Promise` 的語法太久沒用，以致於在一場很期待的線上面試時，突然忘記語法怎麼寫，手忙腳亂地亂查資料然後最後還超時沒寫出來，當場想原地敲昏自己。

網上有太多案例解釋 `Promise` 在幹嘛，這邊就不再贅述，簡單來說就是用 `Promise`建立一個非同步運算的最終完成或失敗的物件，再更簡單一點就是要『發出一個承諾，這件事情做完了(無論成功失敗)，我才要你做下一件事情』，網上最常見的應該就是點餐案例了。

那怎麼用呢？
我們先建立三個 `Prpmise`
```
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('get promise1');
  }, 300);
});
const promise2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('get promise2');
  }, 300);
});
const promise3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('get promise3');
  }, 300);
});
```

## then()爆他

最簡單的應用方式，就是一直 `then()` 下去
```
promise1.then((value) => {
  console.log(value);
  return promise2;
}).then((value) => {
  console.log(value);
  return promise3;
}).then((value) => {
  console.log(value);
})
```
結果應該是
```
"get promise1"
"get promise2"
"get promise3"
```

用這樣的方法應該就可以依序拿到三個 `Promise` 了
要注意如果沒有 `then()` 的話，假設直接呼叫 `console.log("promise1", promise1);`
你只會拿到
```
"promise1" //[object Promise];
{}
```

這是 `Promise` 最基礎的用法，也避免了可怕的 callback hell
![](https://res.cloudinary.com/practicaldev/image/fetch/s--1ppnEIAU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/a79vj0fvvdylajtcqz87.gif)

## 我不在乎順序，我全都要
如果你跟 ***豹頭*** 一樣，不在乎順序，只想全都要的話，那 `Promise.all( […] )` 可能會幫上你

一樣的剛剛那三個 `Promise`
```
function promiseAll (){
  Promise.all([promise1, promise2, promise3])
    .then(function(values) {
      console.log(`${values[0]}, ${values[1]}, ${values[2]}`)
    });
};
promiseAll();
```

意思就是我不在乎誰先誰後，我要你們全都好了再來跟我說 ~~（慣老闆）~~，這樣你應該會拿到一次全好的結果匯報。

```
"get promise1, get promise2, get promise3"
```
## then()還是好醜怎麼辦...(你才醜你全家都醜)

Async / Await 是 ES7 才出現的語法糖，其中一大特點就是讓「非同步的程式碼」讀起來更像在寫「同步程式碼」，跟 `Promise` 基本上做一樣的事，只是經過包裝讓可讀性跟維護性都更好一點。

```
async function asyncFunc (){
  console.log("start async");
  
  const awaitPromise1 = await promise1;
  console.log("await Promise1", awaitPromise1);
  
  const awaitPromise2 = await promise2;
  console.log("await Promise2", awaitPromise2);
  
  const awaitPromise3 = await promise3;
  console.log("await Promise3", awaitPromise3);
};

asyncFunc();
```
然後就會得到跟 `then()` 爆他一樣類似的結果，一個一個依序拿到了

```
"await Promise1" "get promise1"
"await Promise2" "get promise2"
"await Promise3" "get promise3"
```

#### 最後希望我還是能拿到面試邀約嗚嗚嗚

參考資料：
* [codepen 範例](https://codepen.io/wade0615/pen/OJjomEo?editors=0011)
* [MDN 使用 Promise](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Using_promises)
* [MDN async function](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Statements/async_function)
* [你懂 JavaScript 嗎？#24 Promise](https://cythilya.github.io/2018/10/31/promise/)
* [[筆記] 認識同步與非同步 — Callback + Promise + Async/Await](https://medium.com/%E9%BA%A5%E5%85%8B%E7%9A%84%E5%8D%8A%E8%B7%AF%E5%87%BA%E5%AE%B6%E7%AD%86%E8%A8%98/%E5%BF%83%E5%BE%97-%E8%AA%8D%E8%AD%98%E5%90%8C%E6%AD%A5%E8%88%87%E9%9D%9E%E5%90%8C%E6%AD%A5-callback-promise-async-await-640ea491ea64)
