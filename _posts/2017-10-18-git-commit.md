---
layout: post
title: "Git 追蹤與提交"
description: "Git 追蹤紀錄與提交紀錄"
categories: [Git]
tags: [Git, Tools]
redirect_from:
  - /2017/10/18/
---

Git 版本控制 可以紀錄每個提交, 過去沒接觸到版本控制之前只用壓縮檔案備份檔案常常發現一些問題, 除了無法得知某個壓縮檔當時備份時修改過甚麼, 還有檔案多人共用的時候回復檔案後也常常發現資料有衝突不知道哪個檔案最終版本是甚麼, 發現版本控制之後可以解決這些問題, 不只可以多人共同工作每個提交(備份)都會有一筆紀錄, 發現程式檔案毀掉的時候可以用以前提交的紀錄來進行資料回復

## Git 加入追蹤

檔案修改後將加入暫存區, 作法有2種

1. 加入這個目錄下的所有被變更的檔案

	```
	git add .
	```

2. 加入指定檔案

	```
	git add <file_name>
	```

## Git 提交紀錄

工作告一段落之後提交一個紀錄, 這個提交只會紀錄有被追蹤的檔案



1. 提交紀錄, 填入體交資訊

	```
	git commit -m 'your_commit_messenge'
	```

2. 提交紀錄, 使用預設編輯器編輯提交資訊
	
	> 設定預設提交編輯器筆記

	```
	git commit
	```

3. 提交紀錄, 提交所有以前有被追蹤且已經被修改的檔案

	```
	git commit -a
	```

## Git提交範例

建立一個專案並提交
* 建立位置 /home/user/project
* 追蹤一個檔案

1. 建立git

	```
	user@server:~/project$ git init
	```

2. 開發中的一個檔案 

	```
	user@server:~/project$ vi hello.sh        # 建立一個小檔案
	user@server:~/project$ cat hello.sh       # 觀看一下檔案內容
	echo "hello world"
	```

3. 追蹤hello.sh

	```
	# 追蹤前狀態
	user@server:~/project$ git status         # 觀察一下追蹤前的狀態
	On branch master
	
	Initial commit
	
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)
	
	        hello.sh
	
	nothing added to commit but untracked files present (use "git add" to track)

	```

	```
	# 追蹤檔案
	user@server:~/project$ git add hello.sh
	```

	```
	# 追蹤後狀態
	user@server:~/project$ git status         # 觀察一下追蹤後的狀態
	On branch master
	
	Initial commit
	
	Changes to be committed:
	  (use "git rm --cached <file>..." to unstage)
	
	        new file:   hello.sh
	
	```
	
4. 提交紀錄

	提交後會建立一個紀錄, 如果沒有追蹤檔案, 則不會被提交

	```
	user@server:~/project$ git commit -m 'frist commit'
	[master (root-commit) 2a132c5] frist commit
	 1 file changed, 1 insertion(+)
	 create mode 100644 hello.sh

	```

5. 觀看提交紀錄

	```
	commit 2a132c54e6cb96dbad8eb5362d2ffa5e6d91cede
	Author: user <user@example.com>				# 作者
	Date:   Fri Oct 18 09:05:45 2017 +0800		# 提交日期
	
	    frist commit							# 提交的訊息
	
	```
