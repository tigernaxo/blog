---
title: "[.NET Core] ASP .NET Core 3.1 驗證與授權(二)-驗證設定"
date: 2020-11-23T08:40:00+08:00
lastmod: 2020-23-16T08:40:00+08:00
draft: true
tags: ["Authentication", "dotNetCore"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
---
# 驗證方案(Authentication Scheme)
## 概觀
驗證方案包含兩個部分：
- 驗證處理函式(Authentication handler)，負責:
    - 驗證使用者。
    - 當未經驗證的用戶嘗試訪問需驗證資源時作出回應，也就是上一篇提到的 challenge action。
- 驗證處理函式的設定選項(Opitons of Authentication handler)

## 使用驗證方案
在 Startup.ConfigureServices 以 AddAuthentication 註冊驗證服務時會回傳一個 AuthenticationBuilder，
AuthenticationBuilder 設定驗證方案的方式有：
- 呼叫 __scheme-specific 擴充方法__，例如 AddJwtBearer、AddCookie，這些擴充方法會自動呼叫 AuthenticationBuilder.AddScheme 設定需要的驗證方式。
- 以 AuthenticationBuilder __內建方法 AddScheme__ 手動設定，一般來說較少使用。

# 範例
以上 Scheme 的文字較抽象較難理解記憶，以下直接實作註冊 IAuthenticationService，
並直接呼叫 scheme-specific 擴充方法添加 Cookie、JWT Scheme：  

## 安裝 scheme 套件
安裝 scheme 套件取得 Cookie、JWT 的 scheme-specific 擴充方法：
```shell
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer --version 3.1.10
dotnet add package Microsoft.AspNetCore.Authentication.Cookies --version 2.2.0
```
在 Startup.cs 添加對 scheme 的引用
```c#
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.AspNetCore.Authentication.Cookies;
```
## 註冊驗證服務
在 Startup.ConfigureServices 加入下面程式碼，AddAuthentication 會將 IAuthenticationService 驗證服務注入服務容器，
AddJwtBearer、AddCookie 分別為驗證服務添加兩種可用的 JWT、Cookie 驗證方案，
並透過 Action (就是那個 options => ... )設定相關的處理函式與設定選項。  
若 Authorization Attribute 或 Policy 沒有指定 Scheme，預設會使用 AddAuthentication() 方法中傳入作為參數的 Scheme，同理若是服務容器中提供多種驗證服務，可在 Controller 的授權屬性指定套用的 Scheme 或 Policy：
```c#
// 若沒有指定 Scheme 作 Authorization ，預設使用 JwtBearerDefaults.AuthenticationScheme
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    // 讓 AP 可使用 JwtBearerDefaults.AuthenticationScheme 作為驗證方法
    .AddJwtBearer(
        JwtBearerDefaults.AuthenticationScheme,
        options => Configuration.Bind("JwtSettings", options)
        )
    // 讓 AP 可使用 CookieAuthenticationDefaults.AuthenticationScheme 作為驗證方法
    .AddCookie(
        CookieAuthenticationDefaults.AuthenticationScheme,
        options => Configuration.Bind("CookieSettings", options)
        );
```

# Reference
- [MSDN - Overview of ASP.NET Core authentication](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/?view=aspnetcore-3.1)
- [MSDN - Microsoft.AspNetCore.Authentication.Cookies Namespace](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authentication.cookies?view=aspnetcore-5.0)
- [MSDN - Microsoft.AspNetCore.Authentication.JwtBearer Namespace](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authentication.jwtbearer?view=aspnetcore-5.0)
- [Authentication handler in ASP.Net Core (JWT and Custom)](https://dotnetcorecentral.com/blog/authentication-handler-in-asp-net-core/)

https://blog.johnwu.cc/article/ironman-day11-asp-net-core-cookies-session.html
