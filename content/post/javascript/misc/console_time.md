---
title: "[JavaScript] 測量程式執行時間"
date: 2020-11-12T05:41:00+08:00
lastmod: 2020-11-13T06:26:00+08:00
draft: true
tags: ["Javascript", "performance"]
categories: ["JavaScript"]
author: "tigernaxo"

autoCollapseToc: true
---
```js
console.time('someFunction')

someFunction() 

console.timeEnd('someFunction')
```
# Reference 
- [MDN - Console.time()](https://developer.mozilla.org/en-US/docs/Web/API/Console/time)