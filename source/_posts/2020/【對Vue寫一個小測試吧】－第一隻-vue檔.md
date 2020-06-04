---
title: 【對Vue寫一個小測試吧】－第一隻.vue檔
date: 2020-06-02 11:03:52
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E5%B0%8Dvue%E5%AF%AB%E4%B8%80%E5%80%8B%E5%B0%8F%E6%B8%AC%E8%A9%A6%E5%90%A7-%E7%AC%AC%E4%B8%80%E9%9A%BB-vue-%E6%AA%94-53d0a427df2d)

![img](/images/2020/【對Vue寫一個小測試吧】－第一隻.vue檔/vueJest.png)

## 建立檔案

到這邊為止環境算是建置完了，你的結構應該長這樣：
![img](/images/2020/【對Vue寫一個小測試吧】－第一隻.vue檔/BF2I89l.png)

webpack.config.js 裡面應該長這樣：
![img](/images/2020/【對Vue寫一個小測試吧】－第一隻.vue檔/FqJfWQX.png)

接下來我們來建立我們的其他檔案吧～
`touch index.html`
`mkdir components`
`cd components`
`touch index.js`
`touch app.vue`

所以會長的像：
![img](/images/2020/【對Vue寫一個小測試吧】－第一隻.vue檔/8lk9aQF.png)


## vue-SPA

[官網介紹](https://vue-loader.vuejs.org/zh/spec.html)

來編輯 `app.vue` 吧～先放進一點簡單的範例程式：

```
<template>
  <div class="example">{{ msg }}</div>
</template>

<script>
module.exports = {
  data () {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>

<style>
.example {
  color: red;
}
</style>
```
在 webpack.config.js 中新增：
```
// webpack.config.js

module.exports = {
  module: {
    rules: [
      // ... 其它规则
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ]
      }
    ]
  }
}
```
整個變成：
![img](/images/2020/【對Vue寫一個小測試吧】－第一隻.vue檔/hiJhldi.png)

---

這樣對第一隻 vue component 的建置就算是簡單地完成了，下一篇我們來對周圍的 index.html 與 webpack.config.js 再做最後一點點的小修改吧～