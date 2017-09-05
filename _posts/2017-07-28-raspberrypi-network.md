---
layout: post
title: "Raspberry Pi 網路設定"
description: "Raspberry Pi Network Static IP setting"
categories: [Rasbperry-Pi]
tags: [Raspberry-Pi, Linux, Setting]
redirect_from:
  - /2017/07/27/
---

## 查詢網路狀態
觀察一下 Raspberry Pi 上面有那些可以用的網卡
可以用最常見的指令 **ifconfig**

	root@raspberrypi:~ # ifconfig
	eth0      Link encap:Ethernet  HWaddr b8:27:eb:09:91:ed
	          inet addr:192.168.xx.xx  Bcast:192.168.xx.255  Mask:255.255.255.0
	          inet6 addr: fe80::4aec:c77b:6146:1e0c/64 Scope:Link
	          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
	          RX packets:3562 errors:0 dropped:0 overruns:0 frame:0
	          TX packets:1437 errors:0 dropped:0 overruns:0 carrier:0
	          collisions:0 txqueuelen:1000
	          RX bytes:1934821 (1.8 MiB)  TX bytes:192249 (187.7 KiB)
	
	lo        Link encap:Local Loopback
	          inet addr:127.0.0.1  Mask:255.0.0.0
	          inet6 addr: ::1/128 Scope:Host
	          UP LOOPBACK RUNNING  MTU:65536  Metric:1
	          RX packets:282 errors:0 dropped:0 overruns:0 frame:0
	          TX packets:282 errors:0 dropped:0 overruns:0 carrier:0
	          collisions:0 txqueuelen:1
	          RX bytes:22992 (22.4 KiB)  TX bytes:22992 (22.4 KiB)

	

或是用Uinx家族都有的指令 **ip addr**

	root@raspberrypi:~ # ip addr
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
	    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
	    inet 127.0.0.1/8 scope host lo
	       valid_lft forever preferred_lft forever
	    inet6 ::1/128 scope host
	       valid_lft forever preferred_lft forever
	2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
	    link/ether b8:27:eb:09:91:ed brd ff:ff:ff:ff:ff:ff
	    inet 192.168.xx.xx/24 brd 192.168.xx.255 scope global eth0
	       valid_lft forever preferred_lft forever
	    inet6 fe80::4aec:c77b:6146:1e0c/64 scope link
	       valid_lft forever preferred_lft forever
	3: wlan0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
	    link/ether b8:27:eb:5c:c4:b8 brd ff:ff:ff:ff:ff:ff
	    inet6 fe80::1959:afa7:6e4e:85fe/64 scope link tentative
	       valid_lft forever preferred_lft forever

由上面的指令可以得到目前有 eth0 和 wlan0 可以用, eth0 為有線網路, 如果您的網路有dhcp應該就可以自動連線了

## 測試連線
當看到有IP資訊, 網卡也正常工作, 那就使用 ping 來測試一下連線了, 我們測試的目標是Google的伺服器IP是`8.8.8.8`

	root@raspberrypi:~ # ping 8.8.8.8 -c 4
	PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
	64 bytes from 8.8.8.8: icmp_seq=1 ttl=44 time=11.2 ms
	64 bytes from 8.8.8.8: icmp_seq=2 ttl=44 time=10.6 ms
	64 bytes from 8.8.8.8: icmp_seq=3 ttl=44 time=13.8 ms
	64 bytes from 8.8.8.8: icmp_seq=4 ttl=44 time=12.8 ms
	64 bytes from 8.8.8.8: icmp_seq=5 ttl=44 time=11.8 ms
	
	--- 8.8.8.8 ping statistics ---
	5 packets transmitted, 5 received, 0% packet loss, time 4006ms
	rtt min/avg/max/mdev = 10.668/12.089/13.874/1.160 ms



## 設定檔
Raspbian的網路設定檔都存放在 /etc/network 這個目錄中, 我只要設定有線網路 eth0 的網路

	# 切換到/etc/network
	root@raspberrypi:~ # cd /etc/network

	# 確認我們在這裡
	root@raspberrypi:/etc/network # pwd
	/etc/network

主要的設定檔是**interfaces**, 因為現在是使用超級使用者在設定檔案, 為了以防萬一先做個備份檔案

	# 複製一個備份
	root@raspberrypi:/etc/network # cp interfaces interfaces.bk

	# 當你把檔案搞砸了, 做這個動作回復
	root@raspberrypi:/etc/network # rm interfaces
	root@raspberrypi:/etc/network # cp interfaces.bk interfaces

編輯設定檔 interfaces

	# 編輯設定檔
	root@raspberrypi:/etc/network # vi interfaces
	---
	# interfaces(5) file used by ifup(8) and ifdown(8)
	
	# Please note that this file is written to be used with dhcpcd
	# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'
	
	# Include files from /etc/network/interfaces.d:
	source-directory /etc/network/interfaces.d
	
	auto lo
	iface lo inet loopback
	
	# auto eth0						  # 因為我不使用自動IP 因此註解這行
	iface eth0 inet manual			   # manual 表示使用固定IP
	        address 192.168.xx.xx		# 你的IP	(固定的) xx 請更換你的IP數字
	        netmask 255.255.255.0		# 子網路遮罩
	
	allow-hotplug wlan0
	iface wlan0 inet manual
	    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
	---

如果對這個設定有疑問, 可以去問 **男人** 喔!!

	# 問男人
	root@raspberrypi:/etc/network # man interfaces
	  一份很大的文件........
	  按q 離開

## 重新啟動網路
當設定檔都設定完之後, 重啟網路載入新設定才算完成

	# 關閉網卡 eth0
	root@raspberrypi:/etc/network # ifconfig eth0 down
	root@raspberrypi:/etc/network # ifdown eth0
	
這時候用 ifconfig 會看不到 eth0 這不用擔心, 因為 eth0 被我們關掉了, 重啟之後就會回來摟

	# 看!! 只剩下 lo
	root@raspberrypi:/etc/network # ifconfig
	lo        Link encap:Local Loopback
	          inet addr:127.0.0.1  Mask:255.0.0.0
	          inet6 addr: ::1/128 Scope:Host
	          UP LOOPBACK RUNNING  MTU:65536  Metric:1
	          RX packets:282 errors:0 dropped:0 overruns:0 frame:0
	          TX packets:282 errors:0 dropped:0 overruns:0 carrier:0
	          collisions:0 txqueuelen:1
	          RX bytes:22992 (22.4 KiB)  TX bytes:22992 (22.4 KiB)

重新啟動 eth0

	# 啟動網卡 eth0
	root@raspberrypi:/etc/network # ifconfig eth0 up
	root@raspberrypi:/etc/network # ifup eth0

來看一下 eth0 是不是回到 ifconfig 的結果中了

	# 看看 eth0 回來了沒
	root@raspberrypi:/etc/network # ifconfig
	eth0      Link encap:Ethernet  HWaddr b8:27:eb:09:91:ed
	          inet addr:192.168.xx.xx  Bcast:192.168.xx.255  Mask:255.255.255.0
	          inet6 addr: fe80::4aec:c77b:6146:1e0c/64 Scope:Link
	          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
	          RX packets:3562 errors:0 dropped:0 overruns:0 frame:0
	          TX packets:1437 errors:0 dropped:0 overruns:0 carrier:0
	          collisions:0 txqueuelen:1000
	          RX bytes:1934821 (1.8 MiB)  TX bytes:192249 (187.7 KiB)
	
	lo        Link encap:Local Loopback
	          inet addr:127.0.0.1  Mask:255.0.0.0
	          inet6 addr: ::1/128 Scope:Host
	          UP LOOPBACK RUNNING  MTU:65536  Metric:1
	          RX packets:282 errors:0 dropped:0 overruns:0 frame:0
	          TX packets:282 errors:0 dropped:0 overruns:0 carrier:0
	          collisions:0 txqueuelen:1
	          RX bytes:22992 (22.4 KiB)  TX bytes:22992 (22.4 KiB)

看到 eth0 成功回來了吧!! 那就可以使用 ping 去試試看有沒有連上網際網路了