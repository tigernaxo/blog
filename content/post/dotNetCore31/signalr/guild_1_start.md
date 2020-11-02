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
本篇會以[官方文件](https://docs.microsoft.com/en-us/aspnet/core/tutorials/signalr?view=aspnetcore-3.1&tabs=visual-studio)為主，保留必要的部分，並添加一些說明文字，並以 Linux、VS Code 作為開發環境，如果未特地註明都是 bash 指令。

# 建立 SignalR 專案
官方範例中使用到 Razor Pages 的語法，所以起始一個支援 MVC、Razor Pages 的 webapp 專案：
```shell
# 建立專案
dotnet new webapp -o signalr
# 以 VS Code 打開專案
code -r signalr
```

# 建立 SignalR 中樞
SignalR 的 Hub 中文名稱就叫做中樞，在專案中新增資料夾 Hubs 用來專門存放 Hub 實作類別，並在 Hubs 中新增檔案 ChatHub.cs，內容如下：
```c#
using Microsoft.AspNetCore.SignalR;
using System.Threading.Tasks;

namespace SignalRChat.Hubs
{
    // 這就是所謂的 SignalR 中樞
    public class ChatHub : Hub
    {
        // 這是提供 Client (js)端呼叫的方法，後面是這個方法接受的參數
        public async Task SendMessage(string user, string message)
        {
            // 針對每個以連線的客戶端呼叫 ReceiceMassage 方法，並傳送參數 user、message
            await Clients.All.SendAsync("ReceiveMessage", user, message);
        }
    }
}
```

# 設定 Startup.cs
依照官網的設定，在 Startup.cs 當中新增第11, 28, 55 行：
- `using SignalRChat.Hubs;`:新增對 Hub 的引用。
- `service.AddSignalR()`:將 SignalR 相關的對應註冊在 service container 中。
- `endpoints.MapHub<ChatHub>("/chathub");`:將 ChatHub 中樞綁定到站台的 /chathub 端點。

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

# 安裝前端相依性
## LibMan 
LibMan 程式庫管理員可從獲取前端所需要的相依性，來源包含檔案系統(File System)或是[CDNJS](https://cdnjs.com/)、[jsDelivr](https://www.jsdelivr.com/)、[unpkg](https://unpkg.com/#/) 等等[內容傳遞網路(CDN)](https://en.wikipedia.org/wiki/Content_delivery_network)，如果前端以其他框架(ex: Angular、React、Vue)開發時就需要用其他方式，這裡先用微軟提供的方便工具安裝需要的套件。
```shell
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```
## @microsoft/signalr
以下指令 LibMan 會從[unpkg](https://unpkg.com/)將套件 @microsoft/signalr@latest 安裝到專案下的路徑 wwwroot/js/signalr 當中
```shell
libman install @microsoft/signalr@latest \
  -p unpkg \
  -d wwwroot/js/signalr \
  --files dist/browser/signalr.js \
  --files dist/browser/signalr.min.js
```

# 前端程式碼
## HTML
新增檔案 Pages\Index.cshtml，內容如下：

```html
@page
    <div class="container">
        <div class="row">&nbsp;</div>
        <div class="row">
            <div class="col-2">User</div>
            <!-- 使用者名稱輸入框 -->
            <div class="col-4"><input type="text" id="userInput" /></div>
        </div>
        <div class="row">
            <div class="col-2">Message</div>
            <!-- 訊息輸入框 -->
            <div class="col-4"><input type="text" id="messageInput" /></div>
        </div>
        <div class="row">&nbsp;</div>
        <div class="row">
            <div class="col-6">
                <!-- 送出按鈕 -->
                <input type="button" id="sendButton" value="Send Message" />
            </div>
        </div>
    </div>
    <div class="row">
        <div class="col-12">
            <hr />
        </div>
    </div>
    <div class="row">
        <div class="col-6">
            <!-- 顯示訊息的ul -->
            <ul id="messagesList"></ul>
        </div>
    </div>
<script src="~/js/signalr/dist/browser/signalr.js"></script>
<script src="~/js/chat.js"></script>
```

## JavaScript
```js
"use strict";

var connection = new signalR.HubConnectionBuilder().withUrl("/chatHub").build();

//Disable send button until connection is established
document.getElementById("sendButton").disabled = true;

connection.on("ReceiveMessage", function (user, message) {
    var msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    var encodedMsg = user + " says " + msg;
    var li = document.createElement("li");
    li.textContent = encodedMsg;
    document.getElementById("messagesList").appendChild(li);
});

connection.start().then(function () {
    document.getElementById("sendButton").disabled = false;
}).catch(function (err) {
    return console.error(err.toString());
});

document.getElementById("sendButton").addEventListener("click", function (event) {
    var user = document.getElementById("userInput").value;
    var message = document.getElementById("messageInput").value;
    connection.invoke("SendMessage", user, message).catch(function (err) {
        return console.error(err.toString());
    });
    event.preventDefault();
});
```

# Reference
- https://docs.microsoft.com/en-us/aspnet/core/tutorials/signalr?view=aspnetcore-3.1&tabs=visual-studio
- https://docs.microsoft.com/en-us/aspnet/core/client-side/libman/?view=aspnetcore-3.1
