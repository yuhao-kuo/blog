---
layout: post
title: "MPLAB X IDE install note"
description: "MPLAB X IDE 安裝筆記"
categories: [Embedded]
tags: [Tools, PIC]
redirect_from:
  - /2018/07/19/
---

# Microchip MPLAB X IDE install note

[MPLAB X IDE](http://www.microchip.com/mplab/mplab-x-ide) 是 [microchip](https://microchip.com) 最新一代的開發環境, 根據原廠FAE表示最新版本的MPLAB X IDE 將會開始將原先屬於 Atmel 的 AVR晶片整合進來, 目前搭配XC編譯器可以對PIC系列晶片進行開發

## 安裝流程

* 下載點: [MPLAB X IDE](http://www.microchip.com/mplab/mplab-x-ide)

```
cd Downloads/
tar xfav MPLABX-v4.20-linux-installer.tar
sudo ./MPLABX-v4.20-linux-installer.sh
```

![執行畫面](/blog/images/postimg/mplabx/mplabx01.png)

## 更換風格

MPLAB X IDE 是基於 [Netbeans IDE](https://netbeans.org/) 的依款開發環境, 因此可以直接前往 Netbeans 下載版型來使用

這邊我就用我喜歡的 [Darcula](http://plugins.netbeans.org/plugin/62424/darcula-laf-for-netbeans) 來作筆記

![](/blog/images/postimg/mplabx/darktheme01.png)

下載後, 版型檔案最常見的有2種

* `.zip` 請至 `Tools` / `Options` / `Font & Color` 進行安裝
* `.nbm` 請至 `Tools` / `Plugins` / `Download` 進行安裝

    ![](/blog/images/postimg/mplabx/mplabx03.png)

完成後如下

![](/blog/images/postimg/mplabx/mplabx04.png)
