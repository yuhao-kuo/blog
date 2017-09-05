---
layout: post
title: "安裝MariaDB"
description: "安裝MariaDB"
categories: [MariaDB]
tags: [MariaDB]
redirect_from:
  - /2017/08/28/
---

MariaDB是一個MySQL的分支, 目前是由開源社群維護, MariaDB無法在系統中同時存在, 因此如果使用了MariaDB就無法使用MySQL了

> 請注意!! 此筆記是將mariadb安裝在raspberry pi上, 但在Debian/ubuntu上也可以使用, 另外這個筆記不會記錄MariaDB操作指令

# 安裝 MariaDB

	root@rpi:/ # apt-get install mariadb-server

# 服務啟動/觀察狀態/開機啟動

	# 服務啟動
	root@rpi:/ # systemctl restart mysql

	# 觀察mariadb狀態
	root@rpi:/ # systemctl status mysql

	# 開機啟動
	root@rpi:/ # systemctl enable mysql

# 登入MariaDB

	root@rpi:/ # mysql -u root -p
	Enter Password : <輸入密碼>

# 登出MariaDB

	# 在mariadb shell 登出
	MariaDB [(none)]> exit
	Bye				# <<< 自動產生
	root@rpi:/ #	# <<< 回到bash shell了


# 資料來源
1. [How To Raspberry Pi - MariaDB](https://howtoraspberrypi.com/mariadb-raspbian-raspberry-pi/)
2. [wikipedia - MariaDB](https://zh.wikipedia.org/wiki/MariaDB)
