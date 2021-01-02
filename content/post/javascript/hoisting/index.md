---
title: "[JavaScritpt] 提升(Hoisting)與暫時性死區(TDZ;Temporal Dead Zone)"
date: 2021-01-02T00:40:00+08:00
lastmod: 2021-01-02T00:40:00+08:00
draft: true
tags: ["hoisting", "TDZ", "let", "const", "var", "暫時性死區", "提升", "Temporal Dead Zone"]
categories: ["JavaScript"]
author: "tigernaxo"

autoCollapseToc: true
---
網路上時常見到充滿 function 與 var 的 JavaScript 求值題目，
筆者在釐清 Hoisting 和 TDZ 的觀念前時常覺得答案出乎意料，
雖然現在撰寫程式碼都已經避免使用 var，但是維護 legacy code 還是會用到，因此在這裡做個筆記。
# 先導知識
要理解提升前必須先搞懂 JavaScript 變數的宣告、初始化、賦值。  
JavaScript 跟傳統 OOP 語言一樣，在變數宣告、初始化、賦值等等時機取用變數會獲得不同輸出結果，
不一樣的地方在於這些動作實際發生的時間點並不像程式表面那樣一行一行照順序發生！
取值結果|時機|取值結果|時機|取值結果|時機|取值結果
----|----|----|----|----|----|----
ReferenceError: x is not defined | 宣告<br />(Declaration) | Cannot access 'x' before initialization | 初始化<br />(Initiation) | undefined | 賦值<br />(Assignment) | 變數值
# Hoisting
對學過伺服器端語言(C#、C/C++...)的人來說，預期試圖對未宣告的變數取值會出現 ReferenceError是很正常的事，
在 JavaScript 中也是如此：
```js
console.log(x) // ReferenceError: x is not defined
```
但 JavaScript 把 var 宣告變數放在後面，x 前面對 x 取值就變成 undefine，
但是奇怪，怎麼會這樣呢？明明 JavaScript 在我大宣告前就取值，
怎麼能夠認得 x、而且知道 x 被初始化為 undefined？
```js
console.log(x) // undefined
var x
```
原因在於 javascript 會先程式中的蒐集 var(let/const/function) 宣告並放入對應的作用域，
再執行程式碼，這個特行就如同 var 宣告被提升到前面行數的程式碼中一樣，故稱為提升(Hoisting)，
hoisting 的目的在於
宣告方式 |Hoisting|提升時初始化|備註
----|----|----|----
var |O|有(undefined)|初始化為 undefined
let|O|-|只有 let 宣告的變數會發生沒有被初始化的情況
const|O|不需要|合法的 const 宣告必須包含初始化
function|O|不需要|直接以宣告的函式作為初始值
## 認識 JavaScript 解釋器
宣告：用 var、let、const 等等...
初始化：給定預設值
賦值
JavaScript 是直譯式語言，原來直譯式語言也會先被解釋器編譯，
因此要理解 Hoisting 就必須先理解編譯器如何管理變數的生命週期
## 執行環境 Execution Context
全域
global object
函式參數 

## var vs let/const
var 宣告的變數會被初始化為 undefined，而 let 與 const 的宣告不會被初始化為 undefined
# Function Hoisting
以 Function constructor 初始化的方式函式會被提升，且能直接全域叫用
## note
較常疏忽的一點：錯將 function expression 的賦值當成函式宣告(function declaration)，
兩者的差異：
- __function declaration__:
受惠於函式宣告，第2行 x 同時會被提升且初始化為函式，因此第一行可得到結果'x'；
  ```js
  console.log(x()) // 'x'
  function x(){
    return 'x'
  }
  ```
- __function expression__:
第3行以 function expression 得到的 function reference 作為右值賦值給 x，
x 提升的行為就像一般以 var 宣告的變數一樣，提升時只會被初始化為是 undefine 直到第3行才獲得賦值，
因此第3行後才能透過 x 呼叫所指涉的函示，下面第1行可看到時 x 是 undefined，第2行嘗試將 undefined 視為函式呼叫獲得錯誤：
  ```js
  console.log(x) // undefined
  console.log(x()) // Uncxught TypeError: x is not a function
  var x = function(){
    return 'x'
  }
  ```

總歸一句：就像 MSDN 中也強調的 Only declarations are hoisted，function expression assignment 無法喔。
# TDZ
let/const 會發生提升，但 let 的提升不會初始化為 undefined
# Note
- [Medium - Javascript執行環境 (Execution Context)簡介](https://medium.com/digital-dance/javascript%E5%9F%B7%E8%A1%8C%E7%92%B0%E5%A2%83-execution-context-%E7%B0%A1%E4%BB%8B-672185ed6bf4)
- [TechBridge 技術共筆部落格 - 我知道你懂 hoisting，可是你了解到多深？](https://blog.techbridge.cc/2018/11/10/javascript-hoisting/)
- [MSDN - Hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)