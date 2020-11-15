---
title: "[Auth] 使用者名稱/角色與他們的產地 "
date: 2020-11-16T06:19:00+08:00
lastmod: 2020-11-16T06:19:00+08:00
draft: true
tags: ["Authentication", ".NetCore", "Authurication"]
categories: ["NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'
---
# User.Identity.Name 來自何處？
IPrinciple user 物件的屬性
可能來自 FormsAuthenticationModule、Token
# .NET Framework 4.8 當中的 FormsAuthenticationTicket
在 .NET Framework 4.8 當中，以 [FormsAuthenticationTicket](https://docs.microsoft.com/en-us/dotnet/api/system.web.security.formsauthenticationticket?view=netframework-4.8) 作為 value 的 cookie

Name 記載使用者識別名稱(User Identity)
userData 記載以 `|` 分隔的使用者角色 Role

# Reference
- [](https://www.xspdf.com/resolution/52161143.html)