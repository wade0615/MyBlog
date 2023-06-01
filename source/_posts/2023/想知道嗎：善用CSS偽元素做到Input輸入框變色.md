---
title: 想知道嗎：善用CSS偽元素做到Input輸入框變色
date: 2023-05-31 09:16:36
tags:
category: 想知道嗎
---

## 前言

說到監聽網頁前端的 input 有沒有輸入值，來改變輸入匡顏色提醒使用者的需求
想必稍微有用過框架的前端開發者們都會想到用監聽表單 value 是不是 length 為 0 來增減一個 class，去做到改變樣式
無論是利用 React 的 [useEffect](https://react.dev/reference/react/useEffect)，或是 Vue 的 [:class binding](https://vuejs.org/guide/essentials/class-and-style.html#binding-html-classes)
或是其他方式

但這些方式都需要撰寫 JS 來做到表單監聽，去觸發改變樣式行為
有可以不寫 JS 的方式嗎？

有的，如果只是改變樣式，現在已經可以用純 CSS 做到

## 情境

再重複一次情境：

依據實際開發經驗，要變色的區域有可能是 `<input>` 本身，也可能因為有 icon 的關係，要變色的區域是外層的 `<div>`
我的 `<input>` element 可能長這樣

```
<input type="text">
```

也可能長這樣

```
<div>
  <input type="text">
  <span>把我想像成 icon</span>
</div>
```

要做出一種方便達到「監聽網頁前端的 input 有沒有輸入值，來改變輸入匡顏色提醒使用者的需求」的程式撰寫方式

## CSS 解法

在想改變顏色的 element 加上 `is_input_valid` 的 class 即可

### HTML

```
<h2>第一個輸入框</h2>
<input type="text" required class="border_style is_input_valid">

<h2>第二個輸入框</h2>
<div class="border_style parent_dev is_input_valid">
  <input type="text" required>
  <span>把我想像成 icon</span>
</div>
```

### SCSS

```
.border_style {
  border: 2px solid black;
}
.is_input_valid {
  &:required:valid,
  &:has(input:required:valid) {
    border-color: forestgreen;
  }
  &:required:invalid:not(:placeholder-shown),
  &:has(input:required:invalid:not(:placeholder-shown)) {
    border-color: crimson;
  }

  &:focus {
    outline: 0;
  }
  input,
  input:focus {
    border: none;
    outline: 0;
  }
}

.parent_dev {
  display: inline-block;
}
```

## 分開解釋

input element 本身先加了 required 屬性之後

- [:required](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:required)
  > 可以選到擁有 required 屬性的 `<input>` `<select>` `<textarea>`
- [:valid](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:valid)
  > 若 `input` 或其他 `form` 元素 **_驗證正確_**，則有效
- [:invalid](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:invalid)
  > 上一個反過來，**_驗證失敗_**，則有效
- [:has()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:has)
  > 這比較複雜，若 `()` 內所指定的 element 條件，在當前賦予此偽類的子層內有達成，則整個 `:has()` 條件成立，會對賦予此偽類的 element 改變其 CSS 樣式
- [:not()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:not)
  > 防止特定元素被選中，或是不具有某些條件的元素，注意其擁有很多 **_奇招_**
- [:placeholder-shown](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:placeholder-shown)
  > 算是最關鍵的一個，表示當前 **_擁有 placeholder_** 的 `<input>` 或是 `<textarea>`

## 綜合解讀

- :required:valid
  > **_擁有 required_** 且 **_通過驗證_** 的 `<input>`
- :required:invalid:not(:placeholder-shown)
  > **_擁有 required_** 且 **_不通過驗證_** 而且 **_不擁有 placeholder_** 的 `<input>`

再加上 `:has()` 就可以偵測子元素去改變當前元素的 CSS 了
