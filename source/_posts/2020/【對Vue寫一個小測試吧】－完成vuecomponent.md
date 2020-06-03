---
title: 【對Vue寫一個小測試吧】－完成 vue component
date: 2020-06-03 17:14:51
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E5%B0%8Dvue%E5%AF%AB%E4%B8%80%E5%80%8B%E5%B0%8F%E6%B8%AC%E8%A9%A6%E5%90%A7-%E5%AE%8C%E6%88%90-vue-component-598029a39dd5)

![img](/images/2020/【對Vue寫一個小測試吧】－完成vuecomponent/vueJest.png)

上一篇已經編輯好了我們的 vue component ，今天來完成最後的設定吧！

## 編輯其他檔案

### 編輯 index.js

輸入 index.js：
```
import app from './app.vue'

Vue.config.productionTip = false //阻止啟動生產訊息

new Vue({
    el: '#vue_spa',
    template: 
    `<div>
        <app></app>
    </div>`,
    components: { 
        app: app 
    } //這邊要跟你 import 進來時宣告的變數一樣
})
```

### 先跑一次 Webpack

在 run Webpack 之前，要先改一下路徑喔！

將 webpack.config.js 裡的 `entry:` 改成 `"./components/index.js"`
變成：

![](https://i.imgur.com/yqCi26i.png)

這樣才能正確引入我們想轉譯的 .js 喔～

接著這時候先 run 一次 webpack，讓 webpack 幫我們產生 `_dist_` 跟 `buid.js`

`npm run build`

我們的目錄底下應該會多一個 `dist` 的資料夾，裡面有一個 `build.js` 的檔案
![](https://i.imgur.com/igatSgu.png)

這個 build.js 就是 webpack 幫我們把 index.js 裡面，所有我們用的開發用的 JS 語法，像是commonJS的語法（require）、ES6的語法(const)、.vue 的 component 等等，轉譯成瀏覽器知道的ＪＳ，讓瀏覽器可以運行。

### 編輯 index.html

主要就是在 body 新增這三個：
```
//這是我們在 index.js 裡指定要使用 vue 的 element id
<div id="vue_spa"></div>

//引入 vue CDN
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

//引入 Webpack 幫我們轉譯過的 build.js
<script src="./dist/build.js"></script>
```

### 大功告成

點開 index.html 應該就會有類似這樣的畫面了喔：

![](https://i.imgur.com/68A5U3d.png)

之後有任何的變更都記得要再執行 `npm run build` 更新 webPack 整理的 `build.js` 喔！

---


到這邊我已經最簡化的使用套件，不是必須的東西都省略掉的建置完 vue component 的環境了，下一篇來快速講講整系列重點，我用了什麼來達到用最笨的方法使用 Jest 對 vue component 做 unit test 吧！