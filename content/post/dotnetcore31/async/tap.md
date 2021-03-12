---
title: "[.NET Core] TAP"
date: 2021-03-13T00:07:00+08:00
lastmod: 2021-03-13T00:07:00+08:00
draft: true
tags: ["Task", "TAP" ]
categories: [".NET Core"]
author: "tigernaxo"

autoCollapseToc: true
---
controller 前有沒有加上 async 的不同
The async keyword creates a state machine, which is responsible for managing the returned Task. If the code in an async method throws an exception, then the async state machine captures that exception and places it on the returned Task. This is the expected semantics for a method that returns Task.
 that exception would be raised directly to the caller rather than being placed on the returned Task. These semantics are not expected.
 As a general rule, I recommend keeping the async and await keywords unless the method is trivial and will not throw an exception.
# Reference
- [stackoverflow - Task without async/await in controller action method](https://stackoverflow.com/questions/59823334/task-without-async-await-in-controller-action-method)