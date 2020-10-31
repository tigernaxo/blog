---
title: "[台灣杉] 一鍵登入國網中心台灣杉(Taiwania)"
date: 2019-04-21T09:37:18+08:00
lastmod: 2019-04-21T09:37:18+08:00
draft: true
tags: ["台灣杉"]
categories: ["Biotech"]
author: "tigernaxo"

autoCollapseToc: true
---
台灣杉(Taiwania)是國網中心2018年啟用的超級電腦，登入時密碼必須附加OTP (One-time Password)，國網中心建議取得OTP的方式是從驗證器取得OTP密碼，但這樣的方式每次都需要打開驗證器(手機、chrome插件… etc)，並且需要手動輸入OTP，下面分享我從Windows以及Linux環境下登入台灣杉所使用的一鍵登入方式。

OTP的演算法可粗分為HOTP (HMAC-based OTP)；以及基於HOTP的TOTP (Time-based OTP)，我們不必瞭解演算法細節，只需知道台灣杉採用Base32編碼作為TOTP金鑰(Secret)，時間間格為30秒，每30秒可以根據”Secret”與”當前時間區間”以sha1演算法生成一次性密碼(OTP，或稱為Key)，由於這是單向加密的過程所以無法用時間區間與Key逆推Secret，並且OTP在成功使用一次之後就會被伺服器廢棄，以確保安全性。

# 注意事項
- **校時**  
  由於TOTP演算法依據當前時間區間產生OTP，如果作業系統的時間不正確則會產生錯誤的OTP，在Windows環境之下可以到國家時間與頻率標準實驗室的網頁下載NTP校時軟體，以管理員身分執行進行校時；Linux環境可以用下述指令更新時間並寫入BIOS：
  ```bash
  # 從NTP時間伺服器進行網路校時
  sudo ntpdate time.stdtime.gov.tw
  # 將更新的時間寫入BIOS
  sudo hwclock -w
  ```
- **所有登入行為之間必須間隔30秒(包含使用winSCP、putty或從Linux直接登入)。**
  每個30秒區間會產生一個OTP，而該OTP一旦經過使用就會被伺服器廢棄而無法再用。我曾經連續登入以為程式壞了，但核對iService上面產生的密碼又是一樣的，後來才發現這件事…中間還因為連登失敗太頻繁還被鎖，因此記得間隔30秒以上再登入。
- **Linux要登入台灣衫需要將台灣衫加入know host**，以生醫節點為例:
  ```bash
  ssh-keyscan 140.110.148.14 1>>~/.ssh/known_hosts 2>/dev/null
  ```