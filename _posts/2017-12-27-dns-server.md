---
layout: post
title: "debian dns server 架設"
description: "debian 簡易 dns server 架設筆記"
categories: [Linux]
tags: [Linux, Network, Setting]
redirect_from:
  - /2017/12/27/
---

* Kramdown table of contents
{:toc .toc}

這篇筆記使用 **sudo** 表示使用`super user`權限, 如果您沒有開通sudo這個指令, 請直接切換成`root`即可

# 安裝套件

dns伺服器的套件是 `bind9`

```bash
# 請使用 super user 權限
$ sudo apt-get install bind9
```

假設安裝系統時使用最小安裝, 可能不會有 `nslookup` 這個指令

安裝指令如下

```bash
# 請使用 super user 權限
$ sudo apt-get install dnsutils
```

---

# 環境

* IP網域: 192.168.0.1 ~ 192.168.0.255

* 子網路遮罩: 255.255.255.0

* Gateway: 192.168.0.1

* 網域url: example.com
    * 此網域僅用於虛擬機器測試 如有重複請見諒

* 網域中的裝置
    | 裝置名稱 | IP | 用途 |
    | ------- | --- | --- |
    | gateway | 192.168.0.1 | 對外網路 |
    | dns | 192.168.0.254 | 名稱伺服器 |
    | nas | 192.168.0.253 | 網路硬碟 |
    | server 1 | 192.168.0.10 | 伺服器1 |
    | server 2 | 192.168.0.11 | 伺服器2 |
    | desktop 1 | 192.168.0.50 | 電腦1 |
    | desktop 2 | 192.168.0.51 | 電腦2 |

---

# 正反解設定

`Bind` 主要的檔案放在 **/etc/bind** 中

比較重要的有3個檔案

1. named.conf
2. db.IP
3. db.網域

named.conf 裡面 include 了 3 個檔案

1. named.conf.options
2. named.conf.local
3. named.conf.default-zones

這次架 dns 我使用了 named.conf.local, 在這個檔案中定義網域與對應檔案

```bash
zone "example.com" {
    type master;
    file "/etc/bind/db.example.com";   # 正解資料對應檔案路徑
};

zone "0.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.0.168.192";  # 反解資料對應檔案路徑
};
```

> 注意1: IP要反過來寫

> 注意2: file名稱要和正反解檔案相同

## 正解檔案

這個檔案直接複製 db.local

```bash
$ cp db.local db.example.com
```

根據範本填入IP資料

```bash
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     localhost. root.localhost. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      example.com.
@       IN      A       192.168.0.254
dns     IN      A       192.168.0.254
web     IN      A       192.168.0.254
nas     IN      A       192.168.0.253
desktop2    IN      A       192.168.0.51
desktop1    IN      A       192.168.0.50
server2 IN      A       192.168.0.11
server1 IN      A       192.168.0.10
gateway IN      A       192.168.0.1
```

## 反解檔案

這個檔案直接複製 db.127

```bash
$ cp db.127 db.0.168.192
```

根據範本填入IP資料

```bash
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     dns.example.com. root.dns.example.com. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      dns.example.com.
254     IN      PTR     web.example.com.
1       IN      PTR     gateway.example.com.
10      IN      PTR     server1.example.com.
11      IN      PTR     server2.example.com.
50      IN      PTR     desktop1.example.com.
51      IN      PTR     desktop2.example.com.
253     IN      PTR     nas.example.com.
```

---

# 啟動服務

目前比較常見的系統服務有2種

1. system V
2. system D

這裡只紀錄system D

```bash
# 請使用 super user 權限
$ sudo systemctl restart bind9
```

啟動之後來檢查一下狀態

```bash
# 這裡使用一般使用者即可即可
$ systemctl status bind9
# 以下省略
```

---

# 驗證

一旦設定完成可以開始驗證dns功能了

## 正查

```bash
$ nslookup dns.example.com

# 結果如下
Server:         192.168.0.254
Address:        192.168.0.254#53

Name:   dns.example.com
Address: 192.168.0.254


```

## 反查

```bash
$ nslookup 192.168.0.254

# 結果如下
Server:         192.168.0.254
Address:        192.168.0.254#53

254.0.168.192.in-addr.arpa       name = dns.example.com.
254.0.168.192.in-addr.arpa       name = web.example.com.
```

# 後記

1. 要允許dns通過防火牆, 否則只有dns自己可以用這個服務
2. 網域IP順序問題, named.conf.local的zone網域一定要寫對, 不然會讀不到東西

# 參考資料

* [鳥哥的Linux私房菜 DNS伺服器](http://linux.vbird.org/linux_server/0350dns.php)
