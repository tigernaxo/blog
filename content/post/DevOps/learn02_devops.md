---
title: "[DevOps] 初學開發維運-02容器"
date: 2021-10-15T11:11:00+08:00
lastmod: 2021-10-15T11:11:00+08:00
draft: true
tags: ["docker", "k8s", "CI/CD"]
categories: ["DevOps"]
author: "tigernaxo"

autoCollapseToc: false
---

# 容器管理平台
目前最主流的容器軟體是 Docker，容器管理平台是 K8S(Kubernetes)

當負載增加就需要擴充硬體資源來因應，
把應用程式部屬到兩台以上的
微服務 + Kubernets 

# 回到 DevOps
傳統流程大約是這樣子的： (todo 修改)
程式開發：由程式設計師開發程式。
測試：開發到一個階段由開發者或測試人員測試、撰寫測試報告。
系統部屬上線：寫腳本建立程式環境、部屬程式。
維運：軟體環境調校和軟體維護作業。

DevOps是什麼？
傳統的產品開發、測試、維運各自作業，
每次程式開發上線的工作流程大同小異(測試、部屬...)且必須人工作業，

DevOps 透過微服務架構整合 Development(開發)、Operation(維運)，
建立自動部屬、自動測試之類的自動化流程，
可大幅縮短開發交付到部屬上線的流程，藉此更精準快速回應市場需求並大幅減少人力成本，
且因為微服務開發框架從根本解決架構管理的複雜問題，
透過自動化流程將維運工程師從瑣碎日常事務當中解放出來，
，讓開發人員可以更專注於應用開發、其他更有價值的工作、學習新技術。

# 硬體
## 硬體資源擴充與服務執行環境隔離

傳統維運的手法？ shell?
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