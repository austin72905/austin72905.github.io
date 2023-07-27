---
title: Github 上實用的小技巧
date: 2023-7-27 11:49
categories: 技術
tags:
    - git
---

# 前言
Github 是一個代碼的存儲庫，上面有許多優秀的開源方案，有時想要搜尋符合自己需求的專案，搜尋結果可能包含多種程式語言，或是代碼庫早已沒有維護。

就算找到了符合自己需求的代碼庫，每次想要查看他的源代碼都需要下載到本地，本次就來介紹一些實用的技巧，提高查看源代碼時的體驗。

# 搜尋代碼庫技巧

## 進階搜索

可以直接搜尋 `https://github.com/search/advanced`，進入進階搜索的頁面，

也可以直接使用快捷鍵`s`，開啟github 高級搜索功能，可以自行輸入語句設定搜索條件，這邊介紹幾個常用的語法

### 設定星星數

* `mitm stars:>500` (內容包含mitm)

### 設定語言

* `mitm language:Go`
* `mitm language:Go language:JAVA`  (使用 JAVA、Go 寫的)

### 設定代碼庫更新時間，可以用來看存儲庫是否還有在維護

* `mitm pushed:>2020-01-01` 

### 指定欄位

* `mitm in:name`  (指定倉庫名稱包含mitm)

### 同時設定多個條件

* `mitm in:name stars:>500 language:Go  pushed:>2020-01-01`



# 代碼查看技巧

## 存儲庫內常用快捷鍵

### 在代碼庫搜尋文件

使用`T`快捷鍵，可以在代碼庫進行全文件搜索，在那種目錄包很多層的代碼庫中尤其好用

### 快速跳到某一行

使用`L` 快捷鍵

### 查看某文件的改動紀錄

使用`B` 快捷鍵

## 線上開啟vscode 編輯器

使用`.`快捷鍵，github會開啟一個線上vscode 編輯器，就不用每次都要下載源代碼到本地，還可以在線上安裝plugin，但語言的標準庫就不像本地一樣方便，可以直接查看內容

### 使用 alt + <- (左方向建) 可以回到上一頁

這個是vscode 提供的快捷鍵功能


# 線上運行代碼

在網址前墜加上 `gitpod.io/#`

如 
`https://github.com/owner/repository`
改成
`https://gitpod.io/#/github.com/owner/repository`

會開啟gitpod 線上運行環境的功能，自動安裝目前項目需要的依賴包，並可自行輸入command line，在網頁上查看代碼執行的效果，這部分我沒有嘗試，有興趣的可以自行試試看。


# 參考
[5个隐藏的GitHub神技巧，助你秒变大佬！](https://www.youtube.com/watch?v=yoRxfPfFOh8&t=20s)
[21个GitHub精准搜索的神仙技巧](https://zhuanlan.zhihu.com/p/347723938)
[github搜尋語法官方文檔](https://docs.github.com/en/search-github/github-code-search/understanding-github-code-search-syntax)