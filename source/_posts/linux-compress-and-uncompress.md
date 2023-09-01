---
title: Linux 解壓縮、壓縮檔案
date: 2023-9-1 00:41
categories: Linux
tags:
    - Linux
---

# 前言

檔案的打包與壓縮原本是設計用來備份系統文件的，但後來逐漸應用在傳輸大量文件上，將多個文件跟目錄組織在一起傳輸，除此之外也能夠維持目錄結構的完整性，方便程式部屬，本文將介紹一些常用的打包、壓縮文件的指令。

# 打包

打包，只是將多個文件跟目錄，組織到一個文件中(不是目錄)，並沒有進行壓縮

打包過後的檔案，建議以 **<font color=#EB5757>`.tar`</font>** 為文件擴展名，對於後續壓縮、解壓縮時操作較不容易發生錯誤，其他人看到 **<font color=#EB5757>`.tar`</font>** 擴展名，也能知道這是一個被打包過的文件

# 壓縮


壓縮，對於打包過的文件進行壓縮，減少傳輸時文件的大小，建議以 **<font color=#EB5757>`.tar.gz`</font>** 、 **<font color=#EB5757>`.tgz`</font>** 為文件擴展名


# tar (打包檔案、壓縮、解壓縮)

基本上關於文件的打包、壓縮，只要記得這個指令就好

只要用`-z`參數就能調用`gzip`來壓縮了，除非本來只有打包成`.tar`檔，想要把它變成`.tar.gz`，才需要單獨使用`gzip`命令。


常用參數
* **<font color=#EB5757>`-f`</font>** ：<font color=#EB5757>放在參數最後面，後面緊接著打包檔的檔案名稱</font>，一定會使用的參數
* `-c` ：打包檔案
* `-x` ：還原檔案
* **<font color=#EB5757>`-z`</font>** ：調用gzip來`壓縮`or`解壓縮`檔案
* `-v` ：顯示壓縮、打包的執行過程
* `-t` ：不會進行解壓縮，只是可以看到一個壓縮檔裡面的內容

以下是常用範例

## 打包文件 (-c)

* 打包單一個檔案
```bash
    tar -cvf 打包檔名稱.tar 來源檔案 
```

* 打包多個檔案
```bash
    tar -cvf 打包檔名稱.tar 來源檔案1 來源檔案2 ..來源檔案n
```

## 打包並壓縮文件 (-cz)

```bash
    tar -czvf 壓縮檔名稱.tgz 來源檔案 
```

## 解壓文件 (-xz)

* `-c`、`-x` 是相對的，一個是打包檔案，一個是還原檔案，後面接`z`，命令會知道要去`壓縮`還是`解壓縮`

```bash
    tar -xzvf 壓縮檔名稱.tgz 
```

## 查看壓縮檔內的內容 (-t)

* 只是查看檔案內容，沒有進行解壓縮

```bash
    tar -tf 壓縮檔名稱.tgz 
```

# gzip (壓縮、解壓縮文件)

用來解壓縮、壓縮檔案，基本上可以用 tar `-z` 取代

## 壓縮
```bash
    gzip 檔案名稱.tar
```

## 解壓縮
```bash
    gzip -d 檔案名稱.tar.gz
```

# gunzip (壓縮、解壓縮文件，gzip 的硬連結)

gzip 的 硬連結，基本上可以用 `tar -z` 取代

# 參考
[tar 指令的常用語法](https://www.vixual.net/blog/archives/127)

[Linux tar 命令](https://www.runoob.com/linux/linux-comm-tar.html)

[Linux gzip命令](https://www.runoob.com/linux/linux-comm-gzip.html)