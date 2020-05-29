---
title: 【對Vue寫一個小測試吧】－npm & Webpack
date: 2020-05-29 11:12:16
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E5%B0%8Dvue%E5%AF%AB%E4%B8%80%E5%80%8B%E5%B0%8F%E6%B8%AC%E8%A9%A6%E5%90%A7-npm-webpack-e9fc34be9fcf)

![img](/images/2020/【對Vue寫一個小測試吧】－npm&Webpack/vueJest.png)

## npm

首先要先有 npm 的環境：
`npm init -y`

應該大部分開發者都知道 npm 是什麼了吧？簡單來說它就是一個套件管理工具啦～方便我們利用 package.json 管理我們所有的套件，就不再多說啦～

## WebPack

[Webpack官網](https://webpack.docschina.org/)

我們需要 WebPack 幫我們打包轉譯整個我們開發用的 JS
`npm install webpack webpack-cli --save-dev`

### 初步理解

在看了以下前輩的文章之後

Alex Tzeng - Webpack 零設定，入門教學
https://ithelp.ithome.com.tw/articles/10192578

Alex Tzeng - 前端也需要編譯？Transpile、Compile、Minify、Uglify 基本介紹
https://ithelp.ithome.com.tw/articles/10191992

胡立 - webpack 新手教學之淺談模組化與 snowpack
https://blog.techbridge.cc/2020/01/22/webpack-%E6%96%B0%E6%89%8B%E6%95%99%E5%AD%B8%E4%B9%8B%E6%B7%BA%E8%AB%87%E6%A8%A1%E7%B5%84%E5%8C%96%E8%88%87-snowpack/

以下是我節錄整理出我理解的部分：

Webpack 是一支
能把一個 JavaScript 檔案
轉譯成另一個 JavaScript 檔案的程式

因為原生 JavaScript 不支援 像是 `require`, `export` 這些 CommonJS 定義的物件
（node.js 有支援 CommonJS 喔
所以我們寫完一份模組化的 code
實際上，丟到瀏覽器，他是執行不起來的

像是常用的框架 Vue, React, Angular
都會發展出自己特別的語法
(像是 Vue 有 .vue、React 有 .jsx)
這些語法跟 Module 一樣，原生瀏覽器是不認得的
所以我們也會得把他做一次編譯

編譯的過程，也叫做 打包 ，也就是 module bundler（模塊打包工具

整體而言 webpack 是一個非常全能的工具
它提供了許多功能，像是

1. 模組編譯
1. 為 JavaScript 做 minify
1. 編譯 SASS
1. 以及一大堆一大堆⋯⋯

webpack 的好處之一，就是我們能把使用 npm 安裝的模組一併打包，就跟打包自己寫的模組一樣，不需要多做其他事情。這件事是原生的瀏覽器沒辦法做到的。

### 手動建立一個針對 WebPack 的設定檔

`touch webpack.config.js`

在內部設定為：
```
const path = require('path');

module.exports = {
    entry: "./app.js", //引入的檔案

    output: {
        path: path.resolve(__dirname, './dist'), //輸出的路徑
        filename: "build.js" //輸出的檔名
    }
}
```

在 webPack.config.js 裡 require 使用 node.js 原生的 path module
path 這個 module 是 node.js 原生就有提供的
他裡面存有不少路徑轉換的 function來供我們使用

詳情可以參照 Alex 大大的 
webpack.config.js - 設定第二篇 path 和 minify https://ithelp.ithome.com.tw/articles/10193608

### 設定 package.json

在 package.json 的 "script" 裡放入

`"build": "webpack --config webpack.config.js"`

這樣之後只要輸入 `npm run build`
他就會幫我們利用 webpack.config.js 建一個 Webpack 轉譯過的 JS 囉！

---

今天就先到這啦～明天見囉！