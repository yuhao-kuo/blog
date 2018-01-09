---
layout: post
title: "Centos 7 install Odoo"
description: "centos 7 install Odoo"
categories: [Linux, ERP]
tags: [Linux, Setting, ERP]
redirect_from:
  - /2018/08/05/
---

Odoo 是一款開放原始碼的ERP, ERP全名是Enterprise Resource Planning企業資源計劃, 有名的付費ERP有Oracle / SAP / 在台灣比較有名的則是有鼎新等等... Open Souerce的ERP除了Odoo之外還有ERPnext / iDempiere / Dolibarr ERP等等...

本篇的Odoo版本為10, 作業系統為centos 7

## 安裝套件

**odoo**採用的資料庫為**PostgreSQL**, 先裝起來然後啟動吧

```shell
# 安裝postresql
$ sudo yum install -y postgresql-server

# 初始化postgresql
$ sudo postgresql-setup initdb

# 啟動postgresql
$ sudo systemctl enable postgresql
$ sudo systemctl start postgresql
```

接下來就是安裝odoo了, 由於odoo(寫此筆記時)並不在centos的repo中, 所以要先加入repo才能使用yum安裝

```shell
$ sudo yum install epel-release
$ sudo yum install yum-utils

# 加入odoo的repo
$ sudo yum-config-manager --add-repo=https://nightly.odoo.com/10.0/nightly/rpm/odoo.repo

# 安裝odoo
$ sudo yum install -y odoo

# 啟動odoo
$ sudo systemctl enable odoo
$ sudo systemctl start odoo
```

## 連線到 ERP

Odoo預設的port是8069, 開啟瀏覽器 localhost:8069就可以看到了, 當然如果是外部的機器要連入則是需要允許8069通過防火牆

```shell
$ sudo firewall-cmd --perement --add-port=8069/tcp
$ sudo firewall-cmd --reload
```

設定完之後就可以讓用外部電腦連線到 [your erp ip]:8069 看看了
