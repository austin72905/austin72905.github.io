---
title: 查詢 dns 常用指令 (nslookup、dig)
date: 2024-1-3 14:50
categories: 
    - [ Linux]
tags:
    - Linux
    - dns
---


# 常用的 dns 服務器

* `8.8.8.8`：google 的 dns

* `1.1.1.1`：cloudfare 的 dns

* `180.76.76.76`：百度 的 dns

# nslookup

可以用來查詢dns相關訊息，在windows 上也能使用

## 常用範例
* 指定dns服務器查詢
``` bash
    nslookup www.youtube.com 8.8.8.8
```

* 反查
``` bash
    nslookup 123.10.16.8
```

* 指定查詢類型
``` bash
    nslookup -type=mx www.youtube.com 8.8.8.8
```


# dig

dig指令(domain information groper)，可以用來查詢域名的dns相關訊息

## 語法說明

``` bash
    dns 域名

    dns @指定使用的dns服務器 域名

    dig @指定的dns服務器 域名 查詢種類
```

常用參數

* `@dns服務器`：如`8.8.8.8`、`1.1.1.1`，如果沒有指定，預設會使用本地`/etc/resolv.conf` 配置的dns 服務器
* `+short`：簡短回應，只回應ip 或 域名
* `+trace`：顯示域名解析時，經過哪些節點
* `+recurse`：必要時進行遞歸查詢，而不是只返回緩存的結果
* `-x`：反向查詢，通常是輸入IP，可以查詢與之相關的域名
* `-p`：指定訪問dns服務器的port，當dns使用非53port時可用

## 查詢路徑

作業系統會從網域的「最後段」一路反向查詢到「最前段」。例如，以網域 www.ntu.edu.tw 為例，首先會查詢負責回應 **<font color=#EB5757>`tw`</font>** 網域DNS記錄的主機為何，再接著查詢負責 **<font color=#EB5757>`edu`</font>** 網域的主機，而後是負責 **<font color=#EB5757>`ntu`</font>** 網域的主機。最後，再詢問負責 **<font color=#EB5757>`ntu`</font>** 網域的主機，網域名稱 **<font color=#EB5757>`www`</font>** 的主機 IP 為何。也

就是查詢路徑為：

**tw → edu → ntu → www**
 

## 查詢種類
* `A`：ipv4位置
* `AAAA`：ipv6位置
* `ns`：(Name Server) 網域名稱是哪台主機解析的
* `MX`：電子郵件

## 常用範例

* 查詢youtube 域名 指向的 ip
``` bash
    dig www.youtube.com
```

* 使用 google 的 dns 查詢 youtube 域名 指向的 ip
``` bash
    dig @8.8.8.8 www.youtube.com
```

* 詳細輸出
``` bash 
    dig +trace @8.8.8.8  www.youtube.com
```

# 參考
[[鐵人修煉_5]-利用DNS - dig](https://ithelp.ithome.com.tw/articles/10214466)
[DNS設定反查與偵錯：dig指令深入使用](http://mepopedia.com/forum/read.php?135,90293)