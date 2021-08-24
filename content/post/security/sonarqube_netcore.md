---
title: "[Security] 利用免費開源資安檢測軟體 SonarQube 檢測 .NET Core 程式碼"
date: 2021-08-24T10:07:00+08:00
lastmod: 2021-08-24T10:07:00+08:00
draft: true
tags: ["SonarQube", ".Net Core"]
categories: ["Security"]
author: "tigernaxo"

autoCollapseToc: false
#contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'
---
SonarQube 分為兩個部分 SonarQube Server、Scanner (Client 端程式)。
SonarQube Server 提供 Web 使用者介面、搜尋功能、負責處理和儲存分析報告的compute engine，
而 Scanner 可以整合到 CI/CD Server 上進行程式碼品質掃描。
因此整個步驟是 Scanner 掃描完成並將結果上傳 Server，Server 會分析 Scanner 上傳的結果，
分析完成後就可以在 Web 上查看報告，
雖說 localhost 也可以作為 SunarQube Server，
這裡紀錄如何在虛擬機器上實際安裝 SonarQube，
以後在實際機器上安裝就可以作為參考，

## 安裝 SonarQube Server
個人使用或小規模的團隊在單一台機器上安裝 SonarQube 就足夠使用了，
如果需要架設提供大量服務的伺服器，官網也提供 Cluster 的安裝方式方便做 Loading Balance。

首先準備一台 Linux Server，有關 Linux 安裝和環境設置在者裏不是重點不提，
這裡使用 Ubuntu 20.04，
接著下載 Server 端程式
建立資料庫
設置開機啟動
## 安裝 Scanner
## 進行分析
## 上傳分析結果
用 API 上傳資料
## 產生報表?
## 版本控制?
# Referecne
- [SonarQube](https://www.sonarqube.org/)
- [SonarQube GitHub](https://github.com/SonarSource/sonarqube)