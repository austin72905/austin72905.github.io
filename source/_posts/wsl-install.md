---
title: 在win11上使用wsl2
date: 2023-12-16 03:26
categories: Linux
tags:
    - Linux
---

# 前言
wsl 是 windows 操作系統的一個linux子系統，讓你不用像以往一樣需要先下載VirtualBox，才能安裝Linux虛擬機，
wsl 的出現讓Windows 的使用者能夠更簡單的使用 Linux 上那些簡單好用的指令，本文會介紹如何在win11上使用wsl。
注意: win10/win11 預設使用的是wsl2，如果想要使用wsl1 可以去微軟官網查詢相關設定

# 安裝步驟

1. 安裝之前需要先開啟windows 上的功能
    控制台 &rarr; 程式集 &rarr; 開啟或關閉windows功能
    需要將`虛擬機器平台`、`Windows 子系統Linux版` 勾選

![開啟windows上的功能](https://drive.google.com/u/2/uc?id=1jgvN0DyCTJ1RCxhZNgvaaM7O53bygtZ0&export=download)


2. windows update 需要打開接收最新更新通知，之後重新啟動電腦

![打開接收最新更新通知](https://drive.google.com/u/2/uc?id=1dPjPcFOuOGwdnDSYQrrcBOT0WWRYnqVF&export=download)


3. 查看當前系統能夠安裝那些linux 發行版
``` bash
    wsl -l -o
```

4. 安裝Linux 發行版
``` bash
    wsl --install -d <發行版名稱>
```

例如: `wsl --install -d Ubuntu-22.04`

5. 啟動，打開命令行終端，輸入

```
    wsl.exe
```

# 可能出現的錯誤訊息
1. `WslRegisterDistribution failed with error: 0x80370102 Please enable the Virtual Machine Platform Windows feature and ensure virtualization is enabled in the BIOS.`

2. `Error: 0x800701bc WSL 2 ???????????????????? visit https://aka.ms/wsl2kernel`

3. 終端輸入wsl 閃退

基本上就是沒有啟動windows 功能或是沒有打開更新


# 結語
之前用win10時，不記得需要那麼多的設定，win11 微軟號稱把wsl整合進了操作系統中，結果反而在安裝上遇到了很多問題...只能說果然是微軟出品阿。

但是wsl 還是挺不錯的，省略掉很多以前建立虛擬機麻煩的步驟，雖然在某些網路功能上還是不像開一個虛擬機一樣能夠完全的隔離，但一般的文本處理功能也是很夠用了。

# 參考
[WSL2 安裝錯誤 0x800701bc 與 GUI 整合初體驗](https://blog.darkthread.net/blog/wsl2-gui/)

[如何使用 WSL 在 Windows 上安裝 Linux](https://learn.microsoft.com/zh-tw/windows/wsl/install)

[wsl2的Error 0x80370102 解决方案](https://zhuanlan.zhihu.com/p/147233604)