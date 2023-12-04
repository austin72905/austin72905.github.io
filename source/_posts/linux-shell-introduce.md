---
title: shell 基本介紹
date: 2023-11-27 08:35
categories: Linux
tags:
    - Linux
    - shell
---

# 什麼是shell?

shell 英文翻譯又叫外殼，但為什麼叫外殼?總是讓人很疑惑，這時就要先介紹內核了(Linux Kernel)

## 什麼是Linux Kernel?
又稱Linux內核，是整個操作系統的核心部分，負責管理和控制硬體資源，以及提供各種系統服務給應用程式。

由上面的介紹可以知道操作系統是透過內核，來控制底層的硬體設備，但要如何呼叫內核去操作硬體呢?

因此shell 就出現了，目的就是用來呼叫內核的。

為什麼叫shell，因為他是相對於內核，更外層的部分。

# shell 的定義
操控內核的api接口，可以是GUI(圖形化介面)，或者是命令行 (之後文章提到的shell都是這個)

命令行除了單獨執行，也可以組合這些命令行成為腳本，處理比較複雜的流程


# shell 的管理位置

* `/etc/shells` ：可以查看有哪些shell 可以使用


* `/etc/passwd` ： 裡面記載了目前使用的shell 

* `echo $0` ：可以用此指令查看目前使用的shell 

# shell 類型

* GUI (圖形化介面)
    - Gnome
    - KDE
* 命令行
    - sh
    - bash (sh 的升級版，目前主流都是用他)
    - csh、tcsh (很少人用，bash無法在此環境下使用，要注意)
    - ksh(兼容bash、sh，但主要在unix solorias 中使用)