---
layout: post
title: "VirtualBox 橋接網路介面"
description: "VirtualBox 橋接網路介面實作筆記"
categories: [VirtualMachine]
tags: [VirtualMachine, Network]
redirect_from:
  - /2017/12/26/
---

* Kramdown table of contents
{:toc .toc}

VirtualBox 網路模式整體來分有4種

1. NAT
2. 橋接介面卡
3. 內部網路
4. 未附加

而NAT可對外部網路連線, 但對本機則需要透過port轉送的設定才可以完成連線, 內部網路和未附加則是無法對外部網路連線

# 橋接網路介面概念

![橋接網路介面示意圖](/blog/images/postimg/virtualbox-bridge-network/bridgelogic.png)

在實體主機產生一張橋接介面網路卡, 讓虛擬機器經由這個網卡連接上外部網路

# 產生橋接介面

## windows

1. 開啟網路連線
2. 選擇要加入橋接介面的網卡

![建立完成](/blog/images/postimg/virtualbox-bridge-network/brcard.png)

#### 筆記

* 新增網卡
    1. 點選要加入的網卡
    2. [右鍵] 橋接器連線
* 移除網卡
    1. 點選要移除的網卡
    2. [右鍵] 從橋接器中移除

# virtualbox設定

當已經建好橋接網路就可以開始進行虛擬機器的設定了,

設定值 -> 網路 -> 啟用網路卡 -> [附加到]網路介面卡

![設定](/blog/images/postimg/virtualbox-bridge-network/vbox-network.png)

PS. 當名稱的欄位是**未選取**表示bridge沒有設定成功!!

# 挫折紀錄

1. windows下 橋接介面卡無法弄得像Linux bridge那樣可以隨意更改橋接介面的網域
    * 解決方法: 網域使用區域網網段, 可以順利連上網路
        > !!! 但我認為應該還有其他方法可以解決這個問題 想到新的idea再來嘗試看看
2. 虛擬機器網路設定完成, 可以ping通各個區域網的電腦, 但無法連線到外部網路
    * 解決方法: 一時沒察覺在這個網域對外還有一台route, 請網管放行虛擬網卡mac碼就可以順利通行了

# 參考資料

[camel's blog vm建立橋接器教學](https://blog.camel2243.com/2016/09/29/virtualbox-%E6%A9%8B%E6%8E%A5%E7%B6%B2%E8%B7%AF%EF%BC%8C%E8%AE%93-vm-%E8%88%87-host-%E5%9C%A8%E5%90%8C%E4%B8%80%E8%99%9B%E6%93%AC%E7%B6%B2%E6%AE%B5%E4%B8%A6%E5%8F%AF%E9%80%A3%E5%A4%96%EF%BC%8C/)

[virtualbox manual networking mode page](https://www.virtualbox.org/manual/ch06.html)

[Frank's blog vm網路模式差異性](http://ocean2002n.pixnet.net/blog/post/94354066-%5Bvm%5D-%E7%B6%B2%E8%B7%AF%E6%A8%A1%E5%BC%8F-%28host-only%2C-nat%2C-bridge%29-%E5%B7%AE%E7%95%B0%E6%80%A7)

[dr.lee's blog 李老師部落格 Linux建立橋接介面](http://pominglee.blogspot.tw/2014/03/linux.html)

