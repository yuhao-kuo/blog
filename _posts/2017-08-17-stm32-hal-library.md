---
yout: post
title: "STM32 HAL Library 初學筆記"
description: "從std轉換到hal學習筆記"
categories: [STM32]
tags: [STM32]
redirect_from:
  - /2017/08/17/
---

# STM32 HAL Library 初學筆記
以往寫STM32F1時用標準函式庫寫著寫著也沒感覺到甚麼不對勁的地方
直到最近一個小小專案要用到STM32F4突然發現移植過去程式以前Call標準庫的東西不能用了
後來才想到以前寫的程式不懂可移植性的重要, 一直Call底層的東西造成程式的移植困難,
經過Google大神的指引, 發現HAL函式庫可以解決移植性的問題, 因此決定好好學學他

## 開發環境
我使用`GNU ToolChain`搭配Eclipse來工作

## Init 初始化
HAL庫的好處就是初始化部份也幫我考慮進去了, 在stm32f4xx_hal_msp.c中的HAL_MspInit()加入硬體初始化的程式

	stm32f4xx_hal_msp.c

	void HAL_MspInit(void)
	{
		// your code.
		// example: HAL_GPIO_MspInit();
	}

## 作業系統
