---
layout: post
title: "STM32F4 USB Bootloader燒錄 on Linux"
description: "在 Linux 上使用 dfu-util 燒錄 STM32F4"
categories: [STM32]
tags: [STM32, bootloader]
redirect_from:
  - /2017/11/11/
---

兩三個月前寫了一篇windows的燒錄筆記([Link](https://yuhao-kuo.github.io/)), 最近有空也把linux上的dfu燒錄方法做一下紀錄

## 安裝套件

我使用的 Linux 發行版是 ubuntu , 這個燒錄的套件是 `dfu-util`

```
sudo apt-get install dfu-util
```

---

## 燒錄STM32

### STM32進入DFU模式

1. 將版子模式扳到boot
2. 插上usb供電

### 檢查一下usb狀態

為了確保stm32的usb正常運作, 可以透過 `lsusb` 來檢查usb運作的狀態

```
lsusb
```

看到 [STMicroelectronics STM Device in DFU Mode] 表示進入DFU模式了, 成功之後可以進行下一個動作了

```
Bus 001 Device 013: ID 0483:df11 STMicroelectronics STM Device in DFU Mode
```

> 備註: 我插在 USB3.0 的 Port 時常常會斷線, 後來改用 USB2.0 的 Port 就沒問題了

### 燒錄

dfu-util 支援 `.bin` 和 `.dfu` 的燒錄

燒錄資訊

* file: stm32_demo.bin
* usb ID: 0483:df11
* power: usb

```
sudo dfu-util -d 0483:df11 -a 0 -s 0x08000000 -D stm32_demo.bin
```

---

## 上面做的事情

```
user@ubuntu:~$ sudo apt-get install dfu-util
[sudo] password for user:
... 省略 ...
user@ubuntu:~$ cd stm32_demo/
user@ubuntu:~/stm32_demo$ ls
stm32_demo.bin                 # 其他檔案省略
user@ubuntu:~/stm32_demo$ lsusb
Bus 001 Device 013: ID 0483:df11 STMicroelectronics STM Device in DFU Mode
... 省略 ...
user@ubuntu:~/stm32_demo$ sudo dfu-util -d 0483:df11 -a 0 -s 0x08000000 -D stm32_demo.bin
dfu-util 0.8

Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
Copyright 2010-2014 Tormod Volden and Stefan Schmidt
This program is Free Software and has ABSOLUTELY NO WARRANTY
Please report bugs to dfu-util@lists.gnumonks.org

dfu-util: Invalid DFU suffix signature
dfu-util: A valid DFU suffix will be required in a future dfu-util release!!!
Opening DFU capable USB device...
ID 0483:df11
Run-time device DFU version 011a
Claiming USB DFU Interface...
Setting Alternate Setting #0 ...
Determining device status: state = dfuERROR, status = 10
dfuERROR, clearing status
Determining device status: state = dfuIDLE, status = 0
dfuIDLE, continuing
DFU mode device DFU version 011a
Device returned transfer size 2048
DfuSe interface name: "Internal Flash  "
Downloading to address = 0x08000000, size = 4696
Download	[=========================] 100%         4696 bytes
Download done.
File downloaded successfully

# 完成!
```

## 資料來源

[https://wiki.bitcraze.io/projects:crazyflie2:development:dfu](https://wiki.bitcraze.io/projects:crazyflie2:development:dfu)