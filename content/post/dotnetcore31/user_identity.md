---
title: "[.NET Core] ASP .NET Core 3.1 當中的驗證與授權"
date: 2020-11-16T06:19:00+08:00
lastmod: 2020-11-16T06:19:00+08:00
draft: true
tags: ["Authentication", ".NetCore", "Authorization"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'
---

# 驗證(Authentication)
驗證(Authentication) 是確認用戶 Identity 的程序，通過驗證的用戶可具有一或多個 User Identity。  

## Scheme
所謂的 Authentication Scheme 包含兩個部分：
- 驗證處理函式(Authentication handler)，負責:
    - 驗證使用者。
    - 當未經驗證的用戶嘗試訪問需驗證資源時作出回應。
- 驗證處理函式的選項設定(Opitons of Authentication handler)

在 Startup.ConfigureServices 當中註冊驗證服務時設定 Scheme，方法有：
- 呼叫 scheme-specific 擴充方法，例如 AddJwtBearer、AddCookie，這些擴充方法會自動呼叫 AuthenticationBuilder.AddScheme 設定需要的驗證方式。
- 手動以 AuthenticationBuilder.AddScheme 進行設定，一般來說較少使用。

## 註冊驗證服務
以上 Scheme 的文字較抽象較難理解記憶，以下直接實作註冊 IAuthenticationService，並直接呼叫 scheme-specific 擴充方法添加 Cookie、JWT Scheme：  
安裝scheme套件
```shell
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer --version 3.1.10
dotnet add package Microsoft.AspNetCore.Authentication.Cookies --version 2.2.0
```
在 Startup.cs 添加對 scheme 的引用
```c#
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.AspNetCore.Authentication.Cookies;
```
在 Startup.ConfigureServices 加入下面程式碼，AddAuthentication 會將 IAuthenticationService 驗證服務注入服務容器，AddJwtBearer、AddCookie 分別為驗證服務添加 JWT、Cookie 有關的處理函式與設定選項。  
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
## Policy


# 登入 API
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


?? challenge、forbid
?? The Order of UseAuthentication、UseAuthorization
可能來自 FormsAuthenticationModule、Token
# .NET Framework 4.8 當中的 FormsAuthenticationTicket
在 .NET Framework 4.8 當中，以 [FormsAuthenticationTicket](https://docs.microsoft.com/en-us/dotnet/api/system.web.security.formsauthenticationticket?view=netframework-4.8) 作為 value 的 cookie


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