---
title: "[DevOps] 初學開發運維-01"
date: 2021-10-15T11:11:00+08:00
lastmod: 2021-10-15T11:11:00+08:00
draft: true
tags: ["docker", "k8s", "CI/CD"]
categories: ["DevOps"]
author: "tigernaxo"

autoCollapseToc: false
---

# 容器(container)
目前最主流的容器軟體是 Docker，容器管理平台是 K8S(Kubernetes)

## 容器軟體
容器軟體用來運行容器。 
部署容器啟要的資源由容器需要的 CPU、RAM、啟動數量決定，

## 容器管理平台
容器管理平台將每一個可運行容器的 OS(aka Docker 主機) 視為節點(Node)，每個節點的硬體資源可能有所不同，
容器管理平台，管理、調度多個節點的可用資源，讓容器部屬在節點上，
並**掌控**、**維持**容器的運行狀態。
其中最重要的是維持容器的運行狀態，又稱為維持服務級別目標(SLO; Service Level Objective)。
透過這一的機制，使用者只需要告訴容器管理平台容器要運行的容器，不需理會實際各節點資源使用情形、要部屬在哪個節點等等細節...

講白話就是，K8S 根據資源把應用程式適當部屬在不同 OS 內的 Docker，並管理應用程式的運營狀態，在應用程式發生錯誤時進行重啟等等措施。

# DevOps
DevOps，就是自動化**開發(Development)**、**運維(Operations)**、**測試(Quality Assurance)**。

傳統的開發-運維流程大約是這樣子： 
 - 開發：由程式設計師開發程式。
 - 測試：開發到一個階段由開發者或測試人員測試、撰寫測試報告。
 - 部屬：寫腳本建立程式環境、部屬程式。
 - 運維：軟體環境調校和軟體維護作業。


傳統開發與運維人員是分開的兩個腳色，
開發人員的腳色是改變軟體(不論是功能、架構)；
而運維人員最害怕的就是軟體部屬造成的系統不穩定。
並且開發、測試、運維各自作業，但每次程式開發上線的人工作業流程(測試、部屬...)卻大同小異。

DevOps 結合開發與運維，透過自動化串聯開發(交付軟體改變架構/功能)和運維(維持穩定性)流程，
讓整個過程變得更快速可靠。
大幅縮短開發到交付部屬上線的時間
DevOps 建立自動化測試、部屬流程，優點：
 - 更精準快速回應市場需求。 
 - 自動化大幅減少人力成本。
且因為微服務開發框架從根本解決架構管理的複雜問題，
透過自動化流程將運維工程師從瑣碎日常事務當中解放出來，
，讓開發人員可以更專注於應用開發、其他更有價值的工作、學習新技術。

# 硬體
## 硬體資源擴充與服務執行環境隔離

傳統運維的手法？ shell?
#  CICD
上程式的流程自動化

導入現代化 DevOps 帶來什麼好處
方便
安全問題：工程師將編譯的檔案直接放上伺服器，但是程式碼沒有進入版控，會造成混亂。

# Reference
- [第 11 屆 iThome 鐵人賽-就是「懶」才更需要重視DevOps](https://ithelp.ithome.com.tw/users/20120491/ironman/2538)
- [何謂微服務 - 台灣|IBM](https://www.ibm.com/tw-zh/cloud/learn/microservices)
- [medium - 從單體到微服務](https://yunchenli.medium.com/%E5%BE%9E%E5%96%AE%E9%AB%94%E5%88%B0%E5%BE%AE%E6%9C%8D%E5%8B%99-12e206805089)
- [Microservices vs Monolithic Architecture](https://www.mulesoft.com/resources/api/microservices-vs-monolithic)
- [wiki - Multitier architecture](https://en.wikipedia.org/wiki/Multitier_architecture#Three-tier_architecture)
- [wiki - Service-level objective](https://en.wikipedia.org/wiki/Service-level_objective)
- [2017 iT 邦幫忙鐵人賽 - Container 容器三十問](https://ithelp.ithome.com.tw/users/20060041/ironman/1164)