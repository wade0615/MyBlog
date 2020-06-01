---
title: 【對Vue寫一個小測試吧】－vue/cli & vue-Loader
date: 2020-06-01 14:21:33
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E5%B0%8Dvue%E5%AF%AB%E4%B8%80%E5%80%8B%E5%B0%8F%E6%B8%AC%E8%A9%A6%E5%90%A7-vue-cli-vue-loader-864c3dee8ca2)

![img](/images/2020/【對Vue寫一個小測試吧】－vueClivueLoader/vueJest.png)

今天我們要來安裝 vue/cli & vue-loader 了喔～

## vue/cli

[官網介紹](https://cli.vuejs.org/zh/guide/)

CLI (@vue/cli)是一個針對 Vue 的 npm 套件管理工具，提供了終端裡的vue命令。對 Babel、TypeScript、ESLint、PostCSS、PWA、單元測試、E2E測試提供立刻的支援。

### 安裝

`npm install @vue/cli @vue/cli-service-global`

如果你要在全域安裝，你可能需要

`sodu npm install -g @vue/cli @vue/cli-service-global`

你可以在終端輸入： `vue --version` 檢查版本與是否有安裝成功

### 簡介

* `@vue/cli` 包含 `@vue/cli` `@vue/cli-service` `@vue/cli-plugin-`
* 它提供了很多以 `vue` 開頭的終端機裡的命令
* 個人目前常用 `vue serve app.vue` 在開發環境模式下為 app.js 或 app.vue 文件啟動一個 server，瀏覽現在的 component 在頁面中長什麼樣子。
按 ctrl C 離開
* `vue build app.vue` 在開發環境下為 app.js / app.vue 建置一個生產環境的包並用來部署
* `vue create xxxx` 的意思就是他會直接幫你新建一整個包含 `vue/cli` 的專案
* `vue ui` 會用瀏覽器的 ui 介面幫助你完成 `vue create` 的過程
* `vue config` 可以看到或修改 `vue-cli` 的全域設定
    > [vue config](https://cli.vuejs.org/zh/config/#%E5%85%A8%E5%B1%80-cli-%E9%85%8D%E7%BD%AE)是一個可選的配置文件，如果項目的(和package.json同級的)根目錄中存在這個文件，那麼它會被@vue/cli-service自動加載。你也可以使用package.json中的vue字段，但是注意這種寫法需要你嚴格遵照JSON的格式來寫。

### @vue/cli
> CLI (`@vue/cli`)是一個全局安裝的npm包，提供了終端裡的vue命令。它可以通過vue create快速搭建一個新項目，或者直接通過vue serve構建新想法的原型。你也可以通過vue ui通過一套圖形化界面管理你的所有項目。

### @vue/cli-service
> CLI服務( @vue/cli-service)是一個開發環境依賴。它是一個npm包，局部安裝在每個@vue/cli創建的項目中。
CLI服務是構建於webpack和webpack-dev-server之上的。

### @vue/cli-plugin-
> CLI插件是向你的Vue項目提供可選功能的npm包，例如Babel/TypeScript轉譯、ESLint集成、單元測試和end-to-end測試等。Vue CLI插件的名字以@vue/cli-plugin-(內建插件)或vue-cli-plugin-(社區插件)開頭，非常容易使用。

## vue-loader

[官網介紹](https://vue-loader.vuejs.org/zh/#vue-loader-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)

Vue Loader 是一個 webpack 的 loader（這也是為什麼我們前面要先安裝 WebPack 的其中一個原因），它允許我們以一種名為單文件組件(SFCs)的格式撰寫Vue組件
> SFC 就是 Single File Components，就是 SPA - Single Page Application

`npm install -D vue-loader vue-template-compiler`

如果你對為何要裝 `vue-template-compiler` 有疑惑，看看[這個](https://vue-loader.vuejs.org/zh/guide/#vue-cli):
![](https://i.imgur.com/Cqq1uMF.png)

在 webpack.config.js 中新增：
```
// webpack.config.js
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  module: {
    rules: [
      // ... 其他規則
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      }
    ]
  },
  plugins: [
    // 請確保引入這個插件！
    new VueLoaderPlugin()
  ]
}
```

Bonus 補充，選修小知識：
[不需 vue-loader 編譯也能載入 .vue 元件檔: 使用 http-vue-loader](https://kuro.tw/posts/2017/07/11/%E4%B8%8D%E9%9C%80%E7%B7%A8%E8%AD%AF%E4%B9%9F%E8%83%BD%E8%BC%89%E5%85%A5-vue-%E5%85%83%E4%BB%B6%E6%AA%94-%E4%BD%BF%E7%94%A8-http-vue-loader/)

---

東西有點多，很多東西以前沒接觸過我也看了官網文件看了很久，覺得有點複雜跟亂七八糟的話，別緊張，接下來我們先來確認一下目前的 config 檔跟結構都長什麼樣子了吧！