---
layout: post
title: "Linux 架設 http server"
description: "Linux 上架設apache http server"
categories: [Linux]
tags: [Linux]
redirect_from:
  - /2017/09/10/
---
* Kramdown table of contents
{:toc .toc}

本筆記使用的 Apache 當作 http server.

> # 環境參數
> 註: 這些資訊請依照自己實際情況做修改
> 1. 網站路徑: /srv/mypage
> 2. DNS: www.example.com
> 3. 設定檔檔名: 00-mypage.conf

# **RedHat / CentOS**

## [1] 安裝 http 套件
```
root@server:/# yum install httpd
```

## [2] 修改設定檔
* 如果想直接使用預設目錄就直接跳到啟動服務那邊
* 設定檔資料夾路徑 **/etc/httpd/conf.d/**

```
root@server:/# vi /etc/httpd/conf.d/00-mypage.conf
root@server:/# cat /etc/httpd/conf.d/00-mypage.conf
<VirtualHost *:80>
    ServerName www.example.com  # 如果您沒有自己的dns就不需要打
    DocumentRoot /srv/mypage    # 請更換成自己的網站在系統中的實際路徑
</VirtualHost>
<Directory /srv/mypage>
    Require all greanted
</Directory>
```

## [3] 建立網站內容
```
root@server:/# cp /root/mypage /srv/mypage    # 把剛剛設計好的網站複製到跟設定檔相同的位置
```

## [4] 處理SELinux規則
1. 首先必須檢查 SELinux 現在的執行模式
```
root@server:/# getenforce    # 取得SELinux模式
enforcing                    # <<< 當處於這個模式時, SELinux會屏蔽掉違反規則的動作, 請執行第2步驟
```

2. 新增 SELinxu fcontext 規則
```
root@server:/# semange fcontext -a -t httpd_sys_content_t "/srv/mypage(/.*)?"
```

3. 更新 SELinux 規則
```
root@server:/# restorcen -RFvv /srv    # 整個目錄/srv都重新載入規則
root@server:/# ls -lZ /srv             # 查詢檔案的SELinux規則
drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 mypage
#                                       ^^^ 這裡必須跟剛剛設定的規則相同
```

## [5] 啟動HTTP服務
```
root@server:/# systemctl restart httpd    # 重啟 http 服務
root@server:/# systemctl status httpd     # 察看 http 服務
```
* 註: 因為start這個指令在服務某些狀態下會有啟動失敗的問題, 因此個人習慣使用restart

## [6] 開放防火牆 允許HTTP服務通過
```
root@server:/# firewall-cmd --permanent --add-service=http    # 允許通過http
root@server:/# firewall-cmd --reload                          # 重新防火牆規則
```

## [7] 看一下自己網站!!
把瀏覽器開啟來 然後就去看看吧XD

---

# **Debian / Ubuntu**

## [1] 安裝 http 套件
```
root@server:/# apt-get install apache2
```

## [2] 修改設定檔
* 如果想直接使用預設目錄就直接跳到啟動服務那邊
* 設定檔資料夾路徑 **/etc/apache/conf.d**

```
root@server:/# vi /etc/apache/conf.d/00-mypage.conf
root@server:/# cat /etc/apache/conf.d/00-mypage.conf
<VirtualHost *:80>
    ServerName www.example.com  # 如果您沒有自己的dns就不需要打
    DocumentRoot /srv/mypage    # 請更換成自己的網站在系統中的實際路徑
</VirtualHost>
<Directory /srv/mypage>
    Require all greanted
</Directory>
```

## [3] 建立網站內容
```
root@server:/# cp /root/mypage /srv/mypage    # 把剛剛設計好的網站複製到跟設定檔相同的位置
```

## [4] 處理SELinux規則
註: 做這篇Debian/Ubuntu預設是沒有SELinux的, 所以這裡就直接跳過了!!

## [5] 啟動HTTP服務
```
root@server:/# systemctl restart apache2    # 重啟 http 服務
root@server:/# systemctl status apache2     # 察看 http 服務
```
* 註: 因為start這個指令在服務某些狀態下會有啟動失敗的問題, 因此個人習慣使用restart

## [6] 開放防火牆 允許HTTP服務通過

## [7] 看一下自己網站!!
把瀏覽器開啟來 然後就去看看吧XD
