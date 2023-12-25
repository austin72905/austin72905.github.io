---
title: Linux 遠端複製-scp
date: 2023-12-25 22:01
categories: Linux
tags:
    - Linux
    - ssh
---

# 前言
使用ssh 連線遠端時，總是有需要把遠端的文件複製到本地的需求，例如系統崩潰時，想要把日誌抓到本地分析研究，linux 中有個好用的指令 `scp` 可以做到這點。
scp 是基於 ssh 協議的檔案傳輸，預設為`22`port，可以將檔案從 **服務器A** 複製到 **服務器B**。

# 使用方式


## 語法

###  從本地複製文件到遠端 ( 本地 &rarr; 遠端 )

``` bash
    scp 檔案名  遠端服務器IP:遠端服務器目錄

    # 範例
    scp filename.txt austin72905@192.168.56.x:/home/austin72905
```

具體說明

* 遠端服務器IP：格式為 用戶名@ip

* 遠端服務器目錄：複製的檔案要存到遠端服務器的哪個目錄

### 從遠端複製文件到本地 ( 遠端 &rarr; 本地 )

```bash 
    scp 遠端服務器IP:檔案名 本地目錄/ 

    # 範例
    scp austin72905@192.168.56.x:/home/austin72905/filename.txt  /home/myaccount/
```

## 參數

* `-r`：複製目錄時可用
* `-P`：啟用端口號，未指定預設為`port22`
* `-C`：啟用壓縮
* `-i`：用於驗證身分的私鑰文件

# 使用範例

## 指定 port 號

``` bash
    scp -P 2222 file.txt username@remote:/path/to/destination/
```

## 複製整個目錄

``` bash
    scp -r 目錄/ username@remote:/路徑/到/目的地/
```

## 指定驗證身分的私鑰文件

``` bash
    scp -i /path/to/private_key.pem file.txt username@remote:/path/to/destination/
```

