---
layout: post
title: "Linux Pthread API 測試筆記"
description: "Linux Pthread API test"
categories: [Linux]
tags: [Linux, C]
redirect_from:
  - /2017/08/06/
---

近日工作上需要讓程式同時能處理多個工作, 就想起了以前學過的pthread API, 原本使用fork來工作, 但這會比執行緒花掉更多的系統資源, 因此決定來複習一下

## 建立執行緒
