---
layout: post
title: "Odoo change theme"
description: "改變 Odoo 後台管理介面樣式"
categories: [Linux, ERP]
tags: [Linux, Setting, ERP]
redirect_from:
  - /2018/01/09/
---

這次搞的就是改變ERP操作介面的外觀了, 在Odoo的網站上有不少介面是不錯看的, 這篇我使用的是[Material/United Backend Theme
by Openworx](https://www.odoo.com/apps/themes/10.0/backend_theme_v10/), 這是由一家荷蘭Odoo廠商提供的版型, 但請注意這個版本只支援社群版

![](/blog/images//postimg/odoo_theme/odoo_theme_page.png)

## 安裝

1. 下載後將檔案解壓縮放入資料夾中

    * 資料夾位置: /usr/lib/python2.7/site-packages/odoo/addons

    ```
    $ unzip backend_theme_v10-10.0.1.0.23.zip
    ```

    * 解開後會有2個資料夾, 搬過去就可以了
        * web_responsive/
        * backend_theme_v10/

    ```
    $ mv web_responsive /usr/lib/python2.7/site-packages/odoo/addons/.
    $ mv backend_theme_v10 /usr/lib/python2.7/site-packages/odoo/addons/.
    ```

2. 開啟odoo後台, 安裝版型
    1. setting
        ![](/blog/images//postimg/odoo_theme/odoo_theme_setting.png)
    2. Update Apps List
        當設完setting之後, 要更新應用程式表, 讓系統讀取到最新的資料
        * 點下應用程式(Application)
            ![](/blog/images//postimg/odoo_theme/odoo_theme_updatelist.png)
        * 選擇Update即可更新
            ![](/blog/images//postimg/odoo_theme/odoo_theme_updatebtn.png)
    3. 搜尋 theme, 關鍵字`theme`
        ![](/blog/images//postimg/odoo_theme/odoo_theme_search.png)
    4. 找到**Material/United Backend Theme**, 安裝即可
        ![](/blog/images//postimg/odoo_theme/odoo_theme_install.png)
