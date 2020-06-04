---
title: 【對Vue寫一個小測試吧】－Jest Test
date: 2020-06-04 11:56:11
tags:
category: 想知道嗎
---

[<i class="fa fa-medium"></i>](https://medium.com/@wsw0615/%E5%B0%8Dvue%E5%AF%AB%E4%B8%80%E5%80%8B%E5%B0%8F%E6%B8%AC%E8%A9%A6%E5%90%A7-jest-test-2c77f72ff118)

![img](/images/2020/【對Vue寫一個小測試吧】－JestTest/vueJest.png)

建置完最基本的 vue component 之後，我們來使用 Jest 做到最基礎的小測試吧～
我們需要先在 npm 安裝 jest 跟一個叫做 vue-jest 的套件，[Github連結在這](https://github.com/vuejs/vue-jest)

## 安裝

`npm install --save-dev jest vue-jest`

* 在 package.json 裡新增：
```
"jest": {
    "moduleFileExtensions": ["js","json","vue"], //告訴 Jest 處理 *.vue
    "transform": {
      ".*\\.(vue)$": "vue-jest" //用 `vue-jest` 處理 *.vue
    }
  }
```

* 最後再按照 Jest 的基本設定，在 package.json 裡的 `"scripts"` 中新增
`"test": "jest --coverage"`

* 建立 `__test__` 資料夾
`mkdir  __test__`

* 在 `__test__` 中建立 Jest 測試檔
`index.test.js`
    > Jest 在執行測試時，會尋找專案中副檔名為 .test.js 結尾的檔案，但不限制要放在哪個資料夾

* 測試案例：

```
const testingJS = require('../components/app.vue');

test('sum jest', ()=>{
    expect(testingJS.data().msg).toBe('Hello world!');
    expect(testingJS.methods.sum(2, 3)).toBe(5);
})
```

* 執行測試
`npm run test`
應該就會看到類似這樣的畫面囉！
![img](/images/2020/【對Vue寫一個小測試吧】－JestTest/EHoazqC.png)

---

我們終於完成簡單的使用 Jest 對 vue component 做測試的實例，如果想對 Jest 的基礎應用有更多了解，推薦去看看 Titan 的 [Jest：建置測試環境 (包含 Babel)](https://titangene.github.io/article/jest-build-test-env.html)