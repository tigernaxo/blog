---
title: "[.NET Core] ASP .NET Core 3.1 驗證與授權(三)-驗證簽發"
date: 2020-11-23T15:47:00+08:00
lastmod: 2020-11-23T15:47:00+08:00
draft: true
tags: ["Authentication", "dotNetCore", "Authorization"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
---

# 自訂登入 API 
## Identity Objects
User.Identity.Name 來自何處？
## Principal Objects
??IPrinciple user 物件的屬性
IPrincipal 物件帶有 IIdentity 物件的參考
可指定 Authentication Scheme 獲得 Identity
## IAuthenticationService
SignOutAsync 清除 Cookie 的 Claims
在 Cookie 寫入 Claims

# 授權(Authorization)
- 授權(Authorization): 界定用戶可存取資源範圍的程序。
## Policy-based authorization
ASP .NET Core 的授權以政策 Policy 進行設定
## 自訂授權
## RBAC
Name 記載使用者識別名稱(User Identity)
userData 記載以 `|` 分隔的使用者角色 Role

# 驗證與授權
## Challenge、Forbid
## 中間件順序
先驗證、再授權
The Order of UseAuthentication、UseAuthorization



# Reference
- [MSDN - Principal and Identity Objects](https://docs.microsoft.com/en-us/dotnet/standard/security/principal-and-identity-objects)
- [MSDN - IAuthenticationService Interface](https://docs.microsoft.com/zh-tw/dotnet/api/microsoft.aspnetcore.authentication.iauthenticationservice?view=aspnetcore-3.1)
- [MSDN - AuthenticationService Class](https://docs.microsoft.com/zh-tw/dotnet/api/microsoft.aspnetcore.authentication.authenticationservice?view=aspnetcore-3.1)
- [[ASP.NET Core] 加上簡單的Cookie登入驗證](https://dotblogs.com.tw/Null/2020/04/09/162252)
- [Authentication handler in ASP.Net Core (JWT and Custom)](https://dotnetcorecentral.com/blog/authentication-handler-in-asp-net-core/)
