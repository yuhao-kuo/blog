---
layout: post
title: "連接MySQL資料庫"
description: "MySQL基本操作筆記"
categories: [MySQL]
tags: [MySQL]
redirect_from:
  - /2017/08/08/
---
簡易測試指令筆記

## 連線 ##
#### 本機端 ####

	user $ mysql -u root -p
	password: ******

#### 遠端 ####

	user$ mysql -h <ip/dns> -u root -p
	password: ******

## 查詢資料庫列表 ##
	show databases;

## 查詢資料表 ##
在沒有切換到任何資料庫時無法查詢資料表喔!!
	show tables;

## 切換資料庫 ##
	use <db_name>;	/* db_name 改成請自行資料庫名稱 */

## 簡易增刪修查指令 ##
#### 查詢 ####
查詢資料庫中指定表單的所有資料, `db_name`表示資料庫名稱, `table_name`表示資料表單名稱

	mysql> select * from <db_name>.<table_name>;

#### 新增 ####
新增檔案到資料表中, `db_name`表示資料庫名稱, `table_name`表示資料表單名稱


	mysql> insert into <db_name>.<table_name>(欄位名稱1, 欄位名稱2, ...) values(欄位1資料, 欄位2資料, ...);

#### 修改 ####
修改指定資料的內容

	mysql> update <table_name> set <欄位名稱>=<更新的資料> where <欄位名稱>=<要修改的資料>;

#### 刪除 ####
刪除指定資料的內容

	mysql> delete from <db_name>.<table_name> where <欄位名稱>=<要被刪除的資料>;