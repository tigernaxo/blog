---
title: "[SignalR] SignalR Websocket 應用程式(一) - 建立專案"
date: 2020-10-31T02:09:45+08:00
lastmod: 2020-10-31T02:09:45+08:00
draft: true
tags: ["SignalR", ".NetCore", "Websocket"]
categories: [".NET Core 3.1"]
author: "tigernaxo"

autoCollapseToc: true
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'
---

# 建立 signalr 專案
```shell
# 建立專案
dotnet new webapp -o signalr
# 以 vs code 打開
code -r signalr
```

# 安裝 js 相依性
## LibMan 前端套件管理 
LibMan 可以下載前端所需要的相依性，因此先安裝。
```shell
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```
## 安裝 @microsoft/signalr
以下指令 LibMan 會從[unpkg](https://unpkg.com/)將套件 @microsoft/signalr@latest 安裝到專案下的路徑 wwwroot/js/signalr 當中
```shell
libman install @microsoft/signalr@latest \
  -p unpkg \
  -d wwwroot/js/signalr \
  --files dist/browser/signalr.js \
  --files dist/browser/signalr.min.js
```
# 建立 SignalR 中樞 (hub)
在專案中新增資料夾 Hubs，並在 Hubs 中新增一個檔案 ChatHub.cs，內容如下：
```c#
using Microsoft.AspNetCore.SignalR;
using System.Threading.Tasks;

namespace SignalRChat.Hubs
{
    public class ChatHub : Hub
    {
        public async Task SendMessage(string user, string message)
        {
            await Clients.All.SendAsync("ReceiveMessage", user, message);
        }
    }
}
```

# 設定 Startup.cs
在 Startup.cs 當中新增第11, 28, 55 行：
{{< highlight csharp "hl_lines=11 28 55" >}}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.HttpsPolicy;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using SignalRChat.Hubs;

namespace SignalRChat
{
    public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddRazorPages();
            services.AddSignalR();
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Error");
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseStaticFiles();

            app.UseRouting();

            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapRazorPages();
                endpoints.MapHub<ChatHub>("/chathub");
            });
        }
    }
}
{{< /highlight >}}

# Reference
- https://docs.microsoft.com/zh-tw/aspnet/core/tutorials/signalr?view=aspnetcore-3.1&tabs=visual-studio