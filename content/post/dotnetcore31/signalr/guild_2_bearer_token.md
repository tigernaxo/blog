---
title: "[SignalR] SignalR Websocket 應用程式(二) - 授權與驗證"
date: 2020-10-31T02:09:45+08:00
lastmod: 2020-10-31T02:09:45+08:00
draft: true
tags: ["SignalR", ".NetCore", "Authentication", "Autherization", "Bearor Token"]
categories: [".NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'

---

SignalR 的授權可以使用 Cookie 或 Bearer Token，而 Bearer Token 可通用於網頁和 App ，也是官方建議使用的方式  
# Client Side
## javascript 客戶端
在 withUrl 函式當中的參數物件，accessTokenFactory 回傳登入使用的 token：
```js
// Connect, using the token we got.
this.connection = new signalR.HubConnectionBuilder()
    .withUrl("/hubs/chat", { accessTokenFactory: () => this.loginToken })
    .build();
```

# Reference
https://docs.microsoft.com/zh-tw/aspnet/core/signalr/authn-and-authz?view=aspnetcore-3.1
