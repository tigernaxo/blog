---
title: "[SignalR] SignalR Websocket 應用程式(三) - 後端 Token 驗證"
date: 2021-11-08T02:09:45+08:00
lastmod: 2021-11-08T02:09:45+08:00
draft: true
tags: ["SignalR", ".NetCore", "Authentication", "Autherization", "Bearor Token"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'

---

# Server Side
# 驗證 Token
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