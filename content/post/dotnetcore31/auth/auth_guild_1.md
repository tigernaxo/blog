---
title: "[.NET Core] ASP .NET Core 3.1 驗證與授權(一)-驗證與授權"
date: 2020-11-16T06:19:00+08:00
lastmod: 2020-11-16T06:19:00+08:00
draft: true
tags: ["Authentication", "dotNetCore", "Authorization"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
---
在進入 ASP .NET Core 3.1 中驗證(Authentication)與授權(Authorization)的作用流程前，應當對兩者有抽象概念上的認識，以及了解兩者的差異。

# 驗證(Authentication)
驗證是確認用戶識別碼(User Identity)的程序，通過驗證的用戶可具有一或多個用戶識別碼，
因此驗證服務本身就是使用者識別碼提供者 (User Identity Provider)，
ASP.NET Core 3.1 當中以依賴注入(DI; Dependency Injection)將驗證服務注入服務容器 (Service Container)，
使應用程式驗證簽發時能夠取用。

# 授權(Authorization)
授權的作用是界定用戶可存取資源範圍，作用描述如下：
- 限制所存取的資源是否需要驗證。
- 已獲得驗證的特定用戶、特定腳色方能存取特定資源。
- 所存取的資源需要以何種授權政策(Authorizaton Policy)、即驗證方案(Authencation Scheme)。

# 名詞術語
有些名詞需要先解釋：
驗證方案(Authentication Scheme)當中設置了挑戰(Chellange)與禁止(Forbid)應該進行的動作。
## 挑戰(Challenge)
未驗證使用者要存取需驗證才能存取的資源時，授權服務會叫用 ChallengeAsync 發起驗證挑戰
，驗證挑戰一般以 HTTP 回應使用者，且要讓使用者知道應該以哪一種驗證機制取得授權，
驗證挑戰執行的動作常見的具體範例有：
- cookie 驗證方案將使用者轉址到登入頁面。
- JWT 回傳 401 Unauthorized 狀態碼，並在 Header 帶入 `www-authenticate: bearer`。

## 禁止(Forbid)
使用者要存取授權之外的資源。
叫用 ForbidAsync 發起禁止

# 順序關係
按照邏輯來說必須先進行驗證(身分)、再授權(資源)，因此在 Startup.Configure 當中 middleware 應設定如下：
```cs
// 路由對應(route mapping)，必須在授權之前
app.UseRouting();

// 驗證中間件，以服務容器註冊的驗證服務檢查是否有驗證
app.UseAuthentication();
// 授權中間件，從驗證資訊、路由對應、授權設定判斷是否能夠存取所要求的資源
app.UseAuthorization();

// 進入實際資源存取端點
app.UseEndpoints(endpoints =>
{
    // configure endpoints
});
```



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
