---
title: 想知道嗎：“in”與“function in object”
date: 2021-01-31 16:32:06
tags:
category: 想知道嗎
---

![img](/images/2020/想知道嗎：JSforEach的index位置/JS.jpeg =100%x)

有時我只想要知道一個物件中是否擁有某個指定物件，而不需要在當下拿到那個回傳值，告訴我 true 還是 false 就好的時候，有這個 [in](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in)：

```
var trees = new Array("redwood", "bay", "cedar", "oak", "maple");
3 in trees        // 返回true
"bay" in trees    // 返回false (必須使用索引號,而不是陣列元素的值)
var mycar = {make: "Honda", model: "Accord", year: 1998};

"model" in mycar // 返回true
```

對 Array 或 Object 皆通用。

---

再來就是說到可以將函式定義為物件的 Key 這件事了，先假設遇到一個困擾，在不同條件下要執行不同的 function，像[這樣](https://stackoverflow.com/questions/53492013/javascript-refactoring-if-else-statements-es6)：

```
let condition = 'hi';

if(condition === 'hi'){
  commonFunction('hi');

}else if(condition === 'bye'){
  commonFunction('bye');

}else if(condition.includes('happy')){
  commonFunction('happy');

}else if(condition === 'greeting'){
  commonFunction('greeting');

}
```

很明顯如果這樣寫恐怕是有點慘…太多 if 了，解法是可以搭配剛剛的 in ，整個會簡潔很多：

```
const fns = {
  hi() {
    console.log('hi is called');
  },
  bye() {
    console.log('bye is called');
  },
  greeting() {
    console.log('greeting is called');
  },
  happy() {
    console.log('happy is called');
  }
}

const condition = 'hi';

if (condition in fns) {
  fns[condition]();
} else if (condition.includes('happy')) {
  fns.happy();
}
```

藉由 `in` 去判斷 condition 的值是否在 fns 裡面，返回 true 之後再直接呼叫函式出來，看起來好上許多吧！
function in object 的 [MDN 解說](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Functions/Method_definitions)

IE 完全不支援喔！！！