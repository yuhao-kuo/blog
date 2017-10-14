---
layout: post
title: "Command Line 檔案操作"
description: "Linux Command Line 檔案操作筆記"
categories: [Linux]
tags: [Linuz]
redirect_from:
  - /2017/10/14/
---

> 使用Command Line來操作系統中的檔案

---

開始之前, 必須先了解位置表示法中的幾個符號的意義

| 符號 | 意義 |
|-----|-----|
| / | 根目錄 |
| . | 當前目錄 |
| .. | 上一層目錄 |
| ~ | 家目錄 |

* **根目錄 `/`**
    - 檔案系統的最上層稱為根目錄, 所有的目錄樹都是由根目錄開始的
* **當前目錄 `.`**
    - 現在所在位置的目錄, 這個筆記稱為當前目錄, 也就是現在所在的目錄
* **上一層目錄 `..`**
    - 現在位置的上一層目錄, 如果你的當前目錄是根目錄的話, 上一層目錄依然是根目錄
* **家目錄 `~`**
    - 每個使用者個家目錄, 這個目錄使用 `~` 表示, 因此再不同的使用者 `~` 位置都不一樣, 大部份的使用者家目錄都位於 `/home` 中, 但root就是個例外root的家目錄是/root


**絕對路徑**和**相對路徑**的差異
1. 絕對路徑都是由根目錄開始的, 表示開頭都是 `/`
2. 相對路徑由現在位置開始, 舉例來說開頭有 `.` , `..` 或 `~` 就是相對路徑

---

## 檔案查詢

#### 1. `ls` 顯示出目錄中的所有檔案

```
user@server:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

當要找出隱藏檔案時可以加入參數 a

```
user@server:~$ ls -a
.  ..  .ssh  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

想要將所有目錄樹顯示可以加入參數 R

```
user@server:~$ ls -R
.:
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos

./Desktop

./Documents
books.pdf  mydoc.odf

./Downloads

./Music
music.mp3

./Pictures

./Public

./Templates

./Videos
vedio.mp4
```

---

## 目錄操作

#### 1. `pwd` 查詢現在所在目錄

查詢所在目錄的指令是 `pwd` , 這個指令會完整的顯示出目前所在位置的絕對路徑

```
user@server:~$ pwd
/home/user  # 這邊顯示出我們正在user這個目錄中
```

#### 2. `cd` 切換目錄

切換目錄的指令是 `cd` , 這個指令可以為我們切換到我們要求的地方

1. 使用絕對路經切換目錄, 範例: 切換到/tmp目錄

```
user@server:~$ pwd
/home/user              # 現在位置是user的家目錄
user@server:~$ cd /tmp  # 切換到/tmp
user@server:/tmp$ pwd
/tmp                    # 成功切換到/tmp
```

2. 使用相對路徑切換目錄

* 範例1: 使用相對路徑切換到上一層

```
user@server:~$ pwd
/home/user              # 現在位置
user@server:~$ cd ..    # 切換到上一層目錄
user@server:/home$ pwd
/home                   # 現在位置/home
```

* 範例2: 使用 `~` 切換回家目錄

```
user@server:/home$ pwd
/home                   # 現在位置
user@server:/homr$ cd ~ # 切換到上一層目錄
user@server:~$ pwd
/home/user              # 現在位置/home
```

#### 3. `mkdir` 建立目錄

```
user@server:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ mkdir newdir
newdir  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

---

## 檔案/目錄操作

#### 1. `touch` 建立一個空白檔案

檔案不存在時使用`touch`會建立一個檔案

```
user@server:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ touch abc.txt
user@server:~$ ls
abc.txt  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

#### 2. `mv` 移動檔案/目錄

mv 可以用來搬移檔案或搬移目錄, 搬移時可以指定新的名字或是使用`.`來沿用舊名

```
mv <移動的檔案> <新位置or新名字>
```

* 範例1: 將abc.txt搬移到/tmp中
```
user@server:~$ ls
abc.txt  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ mv abc.txt /tmp/.
user@server:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ cd /tmp
user@server:/tmp$ pwd
/tmp
user@server:~$ ls
abc.txt
```

* 範例2: 將abc.txt搬移到/tmp並命名為aaa.txt
```
user@server:~$ ls
abc.txt  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ mv abc.txt /tmp/aaa.txt
user@server:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ cd /tmp
user@server:/tmp$ pwd
/tmp
user@server:~$ ls
aaa.txt
```

#### 3. `cp` 複製檔案

mv 複製時可以指定新的名字或是使用`.`來沿用舊名, 但複製目錄的時候要加上參數R

```
cp <複製目標> <新檔案>
cp -R <複製目標> <新檔案>
```

* 範例1: 將abc.txt複製到/tmp中
```
user@server:~$ ls
abc.txt  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ cp abc.txt /tmp/.
user@server:~$ ls
abc.txt  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ cd /tmp
user@server:/tmp$ pwd
/tmp
user@server:~$ ls
abc.txt
```

* 範例2: 將abc.txt搬移到/tmp並改名為aaa.txt
```
user@server:~$ ls
abc.txt  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ mv abc.txt /tmp/aaa.txt
user@server:~$ ls
abc.txt  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
user@server:~$ cd /tmp
user@server:/tmp$ pwd
/tmp
user@server:~$ ls
aaa.txt
```