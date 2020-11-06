---
title: "[SignalR] SignalR Websocket 應用程式(四) - 前端登入頁面"
date: 2020-10-31T02:09:45+08:00
lastmod: 2020-10-31T02:09:45+08:00
draft: true
tags: ["SignalR", ".NetCore", "Authentication", "Autherization", "Bearor Token"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'

---

# 安裝 axios
SignalR 連線驗證的方式是將 token 夾帶於網址參數中發送到伺服器，因此進行 websocket 連線前我們透過 ajax 向伺服器發送帳號密碼索取登入的 Token，我們先安裝方便使用 ajax 的 axios 函式庫：

axios 一樣可以透過 LibMan 安裝~
```js
libman install axios@latest \
  -p unpkg \
  -d wwwroot/js/axios \
  --files dist/axios.js \
  --files dist/axios.min.js
```
在 Pages/index.cshtml 最後一行添加對 axios 的引用：
```js
<script src="~/js/axios.js"></script>
```

# 登入頁面

# 修改 Chat.js
## 發送 Token
在 withUrl 函式當中的參數物件，accessTokenFactory 回傳登入使用的 token：
```js
// Connect, using the token we got.
this.connection = new signalR.HubConnectionBuilder()
    .withUrl("/hubs/chat", { accessTokenFactory: () => loginToken })
    .build();
```
## 切換頁面