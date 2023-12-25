---
title: 在win11上使用wsl2
date: 2023-12-16 03:26
categories: Linux
tags:
    - Linux
---

# 前言
wsl 是 windows 操作系統的一個linux子系統，讓你不用像以往一樣需要先下載VirtualBox，才能使用Linux作業系統，

wsl 的出現讓Windows 的使用者能夠更簡單的使用 Linux 上那些簡單好用的指令，本文會介紹如何在win11上使用wsl。

注意: win10/win11 預設使用的是wsl2，如果想要使用wsl1 可以去微軟官網查詢相關設定

# 安裝步驟

1. 安裝之前需要先開啟windows 上的功能
    控制台 &rarr; 程式集 &rarr; 開啟或關閉windows功能
    需要將`虛擬機器平台`、`Windows 子系統Linux版` 勾選

![開啟windows上的功能](https://drive.google.com/u/2/uc?id=1jgvN0DyCTJ1RCxhZNgvaaM7O53bygtZ0&export=download)


2. windows update 打開接收最新更新通知，之後重新啟動電腦 (*不一定要開啟，如果在安裝時遇到報錯再開啟就行*)

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

# 輸入wsl 遇到 出現 docker-desktop-root 之類的字眼

檢查一下是否之前安裝過docker desktop (windows 版docker)，docker desktop 會被視為一個linux 發行版

查看已經安裝的發行版
```bash
    wsl --list
```
會發現 有 `docker-desktop`

此時可以切換到另一個發行版

```bash
    wsl --set-version <DistributionName> <VersionNumber>
    # 範例
    wsl --set-version Ubuntu-20.04 2
```

設置默認的發行版
```bash
    wsl --set-default <DistributionName>
    # 範例
    wsl --set-default Ubuntu-20.04 
```

# 與windows 宿主機共用資料夾

在 wsl 終端 進入 `/mnt`，會發現裡面包含著windows 所有的磁碟，接著進入 `/mnt/c`，會看到它顯示的文件跟宿主機上的`C:/`內的文件是一樣的

因此可以在wsl 上 訪問 宿主機上的文件的，但每次都要進入宿主機目錄很麻煩，可以透過建立捷徑(軟連結)的方式

## 建立一個捷徑 (軟連結)
在wsl 上， 先切換到家目錄(比較沒有權限問題)，創建一個目錄`commonWithHost`，之後在此目錄內建立捷徑

假設我想與宿主機共用 `C:\commonWithWSL` (此目錄要存在)

```bash
    cd ~

    mkdir commonWithHost

    # 將宿主機上 C:\commonWithWSL 建立捷徑放在 commonWithHost 目錄裡
    ln -s /mnt/c/commonWithWSL commonWithHost
```
之後進入`commonWithHost` 會發現多了一個`commonWithWSL`，進入後就可以訪問宿主機的文件了


因為是捷徑，所以在wsl 裡面刪掉這個commonWithWSL目錄 沒差，不會影響到windows 上的目錄


# windows 、linux 文件跨平台異常

如果在windows 上編輯文件後，在linux開啟有時會出現以下報錯
<font color=#EB5757>`/bin/bash^M: bad interpreter: No such file or directory`</font>

原因是 不同系統編碼格式引起的：在windows系統中編輯的`.sh`檔案可能有不可見字元，導致跨平台讀取時會有問題


可以用以下方式解決

在linux 環境下，使用 vi 開啟文件，
進入文件後在指令模式下輸入 `:set ff`  ，按下 enter 會看到檔案左下角出現 `fileformat=dos` 或是 `fileformat=unix`。

在linux 環境下 `fileformat=dos` 是會有問題的

輸入`:set ff=unix`，按下 enter 就可以修改，修改後輸入`:wq`保存修改並退出，腳本就可以正常運作了


# 結語
之前用win10時，不記得需要那麼多的設定，win11 微軟號稱把wsl整合進了操作系統中，結果反而在安裝上遇到了很多問題...只能說果然是微軟出品阿。

但是wsl 還是挺不錯的，省略掉很多以前建立虛擬機麻煩的步驟，雖然在某些網路功能上還是不像開一個虛擬機一樣能夠完全的隔離，但一般的文本處理功能也是很夠用了。

# 參考
[WSL2 安裝錯誤 0x800701bc 與 GUI 整合初體驗](https://blog.darkthread.net/blog/wsl2-gui/)

[如何使用 WSL 在 Windows 上安裝 Linux](https://learn.microsoft.com/zh-tw/windows/wsl/install)

[wsl2的Error 0x80370102 解决方案](https://zhuanlan.zhihu.com/p/147233604)

[sh腳本異常：/bin/sh^M:bad interpreter: No such file or directory](https://www.laitimes.com/article/1ebmr_1fxqw.html)