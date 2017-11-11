---
layout: post
title: "STM32F4 USB DFU燒錄 on Windows"
description: "在 Windows 上使用 DfuSe 燒錄 STM32F4"
categories: [STM32]
tags: [STM32]
redirect_from:
  - /2017/08/11/
---

最近實驗室要做教學用的小車子, 所以板子體積要小, 所以就選擇微雪電子的Core405R
這個板子有ST-Link接頭和一個USB接頭, 經費問題弄不到ST-Link剩下USB可以拿來燒錄了
只好跳下去研究USB的Bootloader的燒錄功能了

## Bootloader
Bootloader是一種開機啟動程式, 控制器開機時就會先執行bootloader, 再由bootloader去啟動指定起始位址的應用程式

ST在STM32F405有規劃一塊bootloader程式的區塊, 因此我們可以省略掉燒bootloader程式這個步驟, 直接把程式燒錄進控制器了

## 驅動/燒錄程式
下載ST的bootloader專用燒錄程式
1. [DfuSe USB](http://www.st.com/en/development-tools/stsw-stm32080.html)

## 燒錄步驟
0. **寫隻程式**
	* 我的IO測試程式[Github](https://github.com/yuhao-kuo/demouse)
1. **compiler**
2. **將 Hex檔案 轉成 dfu檔案**
	1. 開啟轉檔工具 **DFU File Manager**
	2. 選擇 轉換DFU檔案<br>
		![](/blog/images/postimg/stm32-usb-bootloader/HexToDfu01.PNG)
	3. 選擇要轉換的Hex檔案<br>
		![](/blog/images/postimg/stm32-usb-bootloader/HexToDfu02.PNG)
		![](/blog/images/postimg/stm32-usb-bootloader/HexToDfu03.PNG)
	4. 轉換!!<br>
		![](/blog/images/postimg/stm32-usb-bootloader/HexToDfu04.PNG)
		![](/blog/images/postimg/stm32-usb-bootloader/HexToDfu05.PNG)
	5. 完成<br>
		![](/blog/images/postimg/stm32-usb-bootloader/HexToDfu06.PNG)
3. **開始燒錄!!**
	1. 開啟燒錄工具 **DfuSe Demo**<br>
		![](/blog/images/postimg/stm32-usb-bootloader/DfuSeDemo01.PNG)
	2. 將開發板設為燒錄模式
	3. 燒錄<br>
		![](/blog/images/postimg/stm32-usb-bootloader/DfuSeDemo02.PNG)
		![](/blog/images/postimg/stm32-usb-bootloader/DfuSeDemo03.PNG)
		![](/blog/images/postimg/stm32-usb-bootloader/DfuSeDemo04.PNG)
		![](/blog/images/postimg/stm32-usb-bootloader/DfuSeDemo05.PNG)
	4. 燒錄完成!!<br>
		![](/blog/images/postimg/stm32-usb-bootloader/DfuSeDemo06.PNG)
