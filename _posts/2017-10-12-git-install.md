---
layout: post
title: "Git 安裝"
description: "Git 版本控制系統安裝筆記"
categories: [Git]
tags: [Git, Tools]
redirect_from:
  - /2017/10/12/
---

[Git](https://git-scm.com) 是現在很流行的版本控制系統, 2005年Linus Torvalds為了更方便的管理[Linux Kernel](https://kernel.org) 開發而設計出來的, 目前由[Git團隊](https://github.com/git)維護中

## Git 安裝

```
# Debian/Ubuntu系列
root@server:~$ apt-get install git

# RedHat/Fedora系列
root@server:~$ yum install git-core
```

## 建立本地端儲存庫

當我們一個專案的目錄需要進行版本控制的時候可以使用`init`指令來建立一個新的儲存庫, 

PS. 此範例使用的目錄是`~/project`

```
user@localhost:~/project$ git init
```

## 下載遠端 Git 儲存庫

當我們要複製一個專案下來本地端進行修改時可以用clone這個指令

```
# 請將clone路徑改為您的路徑
user@server:~$ git https://github.com/yuhao-kuo/test.git
#                  ^^^^^^^^^^^ clone 路徑 ^^^^^^^^^^^^^^
```

