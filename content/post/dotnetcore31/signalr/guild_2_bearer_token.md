---
title: "[SignalR] SignalR Websocket 應用程式(二) - 授權與驗證"
date: 2020-10-31T02:09:45+08:00
lastmod: 2020-10-31T02:09:45+08:00
draft: true
tags: ["SignalR", ".NetCore", "Authentication", "Autherization", "Bearor Token"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'

---

SignalR 的授權可以使用 Cookie 或 Bearer Token：
- Cookie: 驗證方法與一般網頁別無二致，較容易實作但缺點是只能用於瀏覽器(browser-specific)。
- Bearer Token 可通用於網頁和 App (或提供任何應用程式)，使用 Token 做登入能夠讓應用程式更容易實作其他使用者端，如果有其他的伺服器簽發 Token，更容易整合至單一登入(Single Sign-On)，也是官方建議使用的方式，以下假設簽發 Token 與 SignalR 伺服器為同一台進行實作。

# Client Side
SignalR 連線驗證的方式是將 token 夾帶於網址參數中發送到伺服器，因此在進行 websocket 連線之前我們透過 ajax 向伺服器發送帳號密碼索取登入的 Token，我們先安裝方便使用 ajax 的 axios 函式庫：
## 安裝 axios
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
## 修改 Chat.js
在 withUrl 函式當中的參數物件，accessTokenFactory 回傳登入使用的 token：
```js
// Connect, using the token we got.
this.connection = new signalR.HubConnectionBuilder()
    .withUrl("/hubs/chat", { accessTokenFactory: () => loginToken })
    .build();
```

# Server Side
## 簽發Token
### 簽發Token的類別
### 服務容器註冊
###
## 驗證 Token
修改 Configure ，在 ` app.UseAuthorization(); ` 之前加入 ` app.UseAuthorization(); `：
```cs
app.UseAuthentication();
app.UseAuthorization();
```
### 驗證規則
### Hub 驗證屬性
### Hub 解析 Claim

# Reference
- [MSDN - Authentication and authorization in ASP.NET Core SignalR](https://docs.microsoft.com/en-us/aspnet/core/signalr/authn-and-authz?view=aspnetcore-3.1)