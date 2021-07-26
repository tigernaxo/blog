---
title: "[Vue] An Example of Dialog component that returns Promise"
date: 2021-07-24T07:32:00+08:00
lastmod: 2021-07-24T07:32:00+08:00
draft: false
tags: ["Vue", "Promise"]
categories: ["Vue2"]
author: "tigernaxo"

autoCollapseToc: true
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'
---

用法：以外部的元件呼叫視窗元件 getConfirm() 方法，因為 getConfirm 為 Promise 所以可以進行 await 取值等等操作 。

原理：利用 watcher 和 Promise 達成效果。參考 [vue vm-watch api](https://vuejs.org/v2/api/#vm-watch)

應用：Vue router 的 component route guard，在離開表單頁面前跳出使用者確認 dialog

<button id="xBtn">執行測試</button>

# 程式碼 (Code)

```html
<button id="xBtn">執行測試</button>
<div id="xApp" class="modal" :style="{display: dialog?'block':'none'}">
  <div class="modal-content">
    <span class="close">Test Modal</span>
    <p>The value selected will resolve by promise.</p>
    <button @click="choose(1)">1</button>
    <button @click="choose(2)">2</button>
  </div>
</div>
```

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.x/dist/vue.js"></script>
 
<script>
let data = { result: null, dialog: false }
let dialog = new Vue({
  el: '#xApp',
  data:() => data,
  methods: {
   getConfirm() {
     // 先清空 result (避免兩次選中一樣的值無法觸發 watcher)
     this.result = null 
     // open dialog
     this.dialog = true 
     return new Promise((resolve, reject) => {
       try {
         const watcher = this.$watch(
           // 設置監視的對象為 result
           () => this.result ,
           // 一旦 result 的值有改變，就 resolve promise，並啟動下一輪 watcher 
           (newVal) => resolve(newVal) && watcher()
         )
       } catch (error) {
         // 如果出錯就 reject promise
         reject(error)
       }
     })
   },
   choose(value) {
     // 為 result 設置值觸發 watcher 解開 promise
     this.result = value 
     // 關閉 dialog
     this.dialog = false
   }
  }
})
document.getElementById('xBtn')
  .addEventListener( 'click', 
      async e => alert( await dialog.getConfirm() )
    );
</script>
```

```css
/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}
```

<style>
/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}
</style>


<div id="xApp" class="modal" :style="{display: dialog?'block':'none'}">
  <div class="modal-content">
    <span class="close">Test Modal</span>
    <p>The value selected will resolve by promise.</p>
    <button @click="choose(1)">1</button>
    <button @click="choose(2)">2</button>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.x/dist/vue.js"></script>

<script>
let data = { result: null, dialog: false }
let dialog = new Vue({
  el: '#xApp',
  data:() => data,
  methods: {
   getConfirm() {
     // 先清空 result (避免兩次選中一樣的值無法觸發 watcher)
     this.result = null 
     // 打開 dialog
     this.dialog = true 
     // 回傳 Promise
     return new Promise((resolve, reject) => {
       try {
         const watcher = this.$watch(
           // 設置監視的對象為 result
           () => this.result ,
           // 一旦 result 的值有改變，就 resolve promise，並啟動下一輪 watcher 
           (newVal) => resolve(newVal) && watcher()
         )
       } catch (error) {
         // 如果出錯就 reject promise
         reject(error)
       }
     })
   },
   choose(value) {
     // 為 result 設置值觸發 watcher 解開 promise
     this.result = value 
     // 關閉 dialog
     this.dialog = false
   }
  }
})
document.getElementById('xBtn')
  .addEventListener( 'click', 
      async e => alert( await dialog.getConfirm() )
    );
</script>
