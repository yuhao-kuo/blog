---
layout: post
title: "ubuntu 安裝 virt-manager 虛擬機器管理工具"
description: "ubuntu 安裝 virt-manager 虛擬機器管理工具"
categories: [Linux, VirtualMachine]
tags: [Linux, Setting, VirtualMachine, Tools]
redirect_from:
  - /2018/01/12/
---

其實去年已經搞定了這個東西 因為太懶惰一直沒有寫筆記, 最近突然想起來找個小空檔寫了一下筆記

## 環境

首先當然是要介紹一下主角 virt-manager 圖形化虛擬機器管理工具, virt-manager 可以支援Qemu虛擬機器

首先介紹一下 virt-manager 後面的 虛擬機器工具 Qemu, 這是一款支援多平台的虛擬機器, 支援x86, arm等架構的CPU, 再這之前要看一下是本地端的CPU有沒有支援虛擬化的模組

> 因此這裡只有寫Intel的CPU

1. 看一下是否有支援, 如果沒有資料表示你的CPU可能不支援這個功能
    ```
    $ cat /proc/cpuinfo | grep vmx
    flags		: vmx (前後都省略了, 有vmx才是重點)
    ```
2. 檢查一下kvm模組有沒有被載入, 而kvm_intel是intel的kvm模組
    ```
    $ lsmod | grep kvm
    kvm_intel             172032  0
    kvm                   544768  1 kvm_intel
    irqbypass              16384  1 kvm
    ```

最後就是講我的host OS了
* Host OS: ubuntu-mate 16.04 Lts 64bit

再來是CPU要看有沒有

## 安裝

根據 virt-manager 指引可以透過發行版的套件管理工具安裝

```
＃yum install virt-manager（Fedora）
＃apt-get install virt-manager（Debian）
＃emerge virt-manager（Gentoo）
＃pkg_add virt-manager（OpenBSD）
```

我選擇直接下載程式碼進行安裝, 當前版本(2018.01.12)是 `1.4.3`

### 一, 下載程式碼

請到 [官方網站](https://virt-manager.org) 或是 [github](https://github.com/virt-manager/virt-manager) 下載安裝檔案

1. [virt-manager 官方下載頁](https://virt-manager.org/download/)
    1. 下載點位置在左上角, 紅色框框的位置
        ![下載位置](/blog/images/postimg/virt-manager/virt-manager-download-page.png)
    2. 保險起見可以使用gpg去檢查一下下載的檔案是否有問題
2. [git clone 連結](https://github.com/virt-manager/virt-manager.git)
    ```
    $ git clone https://github.com/virt-manager/virt-manager.git
    ```

### 二, 安裝相關套件

```
$ sudo apt-get install qemu
$ sudo apt-get install libvirt-bin
$ sudo apt-get install ubuntu-vm-builder
$ sudo apt-get install bridge-utils
```

### 三, 編譯程式

1. 解壓縮原始碼

    ```
    $ tar xfav virt-manager-1.4.3.tar.gz
    $ cd virt-manager-1.4.3/
    ```

2. 編譯

    ```
    $ ./setup.py build
    ```
3. 安裝到系統中, 記得要使用 super user 權限

    ```
    $ sudo ./setup.py install
    ```

### 使用

* 觀看版本號碼

    ```
    $ virt-manager --version
    1.4.3
    ```

* 啟動virt-manager

    ```
    $ virt-manger # 這樣就可以啟動了
    ```

## 後記

這次安裝的時候發生**virt-manager**運作的時候虛擬機器視窗無法正常顯示機器的畫面也無法建立新的虛擬機器這種奇怪的狀況, 爬了外國的文章發現可以嘗試將qemu也升級到最新版本, 並且重新安裝virt-manager, 這個問題被解決了
