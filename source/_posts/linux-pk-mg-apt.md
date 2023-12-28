---
title: Linux 包管理工具-apt
date: 2023-12-26 00:15
categories: Linux
tags:
    - Linux
---

# 前言

apt 是 Debian 系發行版使用的軟體包管理工具，本質上就是去網路上包的網址，將包下載下來


# apt 與 apt-get 差異

apt-get 是比較舊版本的軟體包管理工具，apt 是近年推出的，支援了一些原本apt-get 沒有的功能，對於處理複雜依賴的關係有加強過

主要差異
* apt upgrade 之後，舊版的套件會從系統刪除
* apt-get upgrade 舊版的套件會保留

# 使用方式


## 列出所有安裝過的包 （apt list）

`apt list`

通常可以搭配grep， 來查詢特定的包是否安裝過

`apt list | grep "nginx"`

## 列出所有安裝過的包及詳細資訊 （apt show）

`apt show`

## 更新包可安裝清單 （apt update）

只更新清單，但不會真的升級

`apt update`

## 升級包 （apt upgrade）

`apt upgrade`

## 查詢包 （apt search）

查詢那些包可以下載

`apt search nginx`

## 安裝包 （apt install）

`apt install nginx`

## 刪除包 （apt remove）

`apt remove nginx`

## 刪除包及設定檔 （apt purge）

`apt purge nginx`
可以清得比較乾淨，但建議是先使用apt remove ，清的不夠乾淨再用purge


## 配置檔 （`/etc/apt/sources.list`）

可以配置每個包的網址，如果想要將第三方軟體也納入apt 管理，可以在此添加三方軟體的下載網址

## 編輯配置檔 （apt edit-sources）

輸入`apt edit-sources`後，會開啟文字編輯器進入 `/etc/apt/sources.list` 編輯畫面

## moo

彩蛋
輸入 `apt moo` 會有一隻牛出現在螢幕上


# 參考

[[Linux] Ubuntu APT 指令更新、升級、移除、安裝套件教學](https://www.tokfun.net/os/linux/install-remove-linux-software-using-apt-command/#%E9%80%8F%E9%81%8E%E3%80%8Capt_edit-sources%E3%80%8D%E6%8C%87%E4%BB%A4%E7%B7%A8%E8%BC%AF%E8%BB%9F%E9%AB%94%E5%BA%AB%E8%A8%AD%E5%AE%9A%E6%AA%94)


[[Linux]apt 和 apt-get 之间有什么区别？](https://zhuanlan.zhihu.com/p/350690109)

[apt 與 apt-get 之間有什麼區別？](https://aws.amazon.com/tw/compare/the-difference-between-apt-and-apt-get/)