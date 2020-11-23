---
title: "[.NET Core] ASP .NET Core 3.1 驗證與授權(三)-驗證簽發"
date: 2020-11-23T15:47:00+08:00
lastmod: 2020-11-23T15:47:00+08:00
draft: true
tags: ["Authentication", "dotNetCore", "Authorization", "Cookie", "Token"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
---
# 認證中的物件
簡單的說，ClaimsPrincipal 可包含很多 ClaimsIdentity(但通常只有一個)；
ClaimsIdentity 可以且通常包含很多 Claims(聲明)，
而每個 Claims 是包含型別(ClaimType)、值(ClaimValue)。

# AuthenticationHttpContextExtensions
AuthenticationHttpContextExtensions 類別對 HttpContext 類別擴展出認證方法，
從服務容器中獲取 IAuthenticationService 實體類別，並調用同名方法。
# IAuthenticationService
SignOutAsync 清除 Cookie 的 Claims
可儲存 ClaimsPrincipal進行簽發(登入)認證，作為身分識別。

# 登入 API 實作
宣告 ClaimsPrincipal 之後，利用在服務容器注入的認證服務(實作 IAuthencationService 的類別)，進行登入、登出。
可指定 Scheme
SignInAsync 需要：
1. ClaimsPrincipal(必要)。
2. AuthenticationScheme string (Optional)
3. authProperties (Optional)
## Cookie 登入
# 自訂登入 API 
## Identity Objects
## Principal Objects
IPrincipal 物件帶有 IIdentity 物件的參考
可指定 Authentication Scheme 獲得 Identity
## IAuthenticationService
SignOutAsync 清除 Cookie 的 Claims
在 Cookie 寫入 Claims
## Token 登入


# Reference
- [MSDN - Principal and Identity Objects](https://docs.microsoft.com/en-us/dotnet/standard/security/principal-and-identity-objects)
- [MSDN - IAuthenticationService Interface](https://docs.microsoft.com/zh-tw/dotnet/api/microsoft.aspnetcore.authentication.iauthenticationservice?view=aspnetcore-3.1)
- [MSDN - AuthenticationService Class](https://docs.microsoft.com/zh-tw/dotnet/api/microsoft.aspnetcore.authentication.authenticationservice?view=aspnetcore-3.1)
- [[ASP.NET Core] 加上簡單的Cookie登入驗證](https://dotblogs.com.tw/Null/2020/04/09/162252)
- [Authentication handler in ASP.Net Core (JWT and Custom)](https://dotnetcorecentral.com/blog/authentication-handler-in-asp-net-core/)
