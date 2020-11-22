---
title: "[.NET Core] ASP .NET Core 3.1 認證與授權(三)-認證簽發"
date: 2020-11-16T06:19:00+08:00
lastmod: 2020-11-16T06:19:00+08:00
draft: true
tags: ["Authentication", "dotNetCore", "Authorization"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
---

# 登入 API 實作
## Identity Objects
User.Identity.Name 來自何處？
## Principal Objects
??IPrinciple user 物件的屬性
IPrincipal 物件帶有 IIdentity 物件的參考
可以指定 Scheme
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

# 認證與授權
## Challenge、Forbid
## 中間件順序
先認證、再授權
The Order of UseAuthentication、UseAuthorization



# Reference
- [MSDN - Principal and Identity Objects](https://docs.microsoft.com/en-us/dotnet/standard/security/principal-and-identity-objects)
- [MSDN - IAuthenticationService Interface](https://docs.microsoft.com/zh-tw/dotnet/api/microsoft.aspnetcore.authentication.iauthenticationservice?view=aspnetcore-3.1)
- [MSDN - AuthenticationService Class](https://docs.microsoft.com/zh-tw/dotnet/api/microsoft.aspnetcore.authentication.authenticationservice?view=aspnetcore-3.1)
- [MSDN - Overview of ASP.NET Core Security](https://docs.microsoft.com/zh-tw/aspnet/core/security/?view=aspnetcore-3.1)
- [MSDN - Overview of ASP.NET Core authentication](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/?view=aspnetcore-3.1)
- [MSDN - Policy-based authorization in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/policies?view=aspnetcore-3.1)
- [MSDN - Microsoft.AspNetCore.Authentication.Cookies Namespace](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authentication.cookies?view=aspnetcore-5.0)
- [MSDN - Microsoft.AspNetCore.Authentication.JwtBearer Namespace](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authentication.jwtbearer?view=aspnetcore-5.0)
- [[ASP.NET Core] 加上簡單的Cookie登入驗證](https://dotblogs.com.tw/Null/2020/04/09/162252)

https://blog.johnwu.cc/article/ironman-day11-asp-net-core-cookies-session.html