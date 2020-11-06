---
title: "[SignalR] SignalR Websocket 應用程式(二) - 後端 Token 授權"
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

# 簽發Token
## 添加控制器支援
在 Startup.cs 的 ConfigureServices 中，將 Controller 支援註冊到服務容器(Service Container)當中:
{{< highlight csharp "hl_lines=3" >}}
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddRazorPages();
    services.AddSignalR();
}
{{< /highlight >}}

## 添加Token套件
```shell
dotnet add package System.IdentityModel.Tokens.Jwt --version 6.8.0
```

## Login 控制器
建立一個檔案