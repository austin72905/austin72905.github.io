---
title: Linux 網路抓包工具-tcpdump
date: 2023-10-7 14:21
categories: 
    - [ Linux]
    - [ 網路]
tags:
    - Linux
    - 網路抓包
---

# 前言
tcpdump 是 linux 常使用的網路抓包工具，可以抓取機器上的網路封包，可以分析網路的使用情況，如查看當前服務使用的dns是否正常響應，或是查看機器是否被駭客開了後門，正在使用奇怪的port號把資料送出去今天就記錄一些常用的tcpdump指令與參數。

# tcpdump


常用參數
* **<font color=#EB5757>`-i`</font>** ：監聽的網路介面卡，通常都是eth0

* `-n` ：不把主機的地址轉換成名字

* `-t` ：每條封包內容不打印時間戳

* `-s` ：設置每條封包的大小為多少bytes

* `-X` ：使用十六進制(hex)顯示每條封包的資料


指定類型(type)
* **<font color=#EB5757>`port`</font>** ：特定端口或服務，如: 80、443 ; 或是指定服務 <font color=#EB5757>`domain`</font>、http、ssh、ftp

* `portrange` ：指定端口範圍，如: 1000-2000

* `net` ：可以指定一個ip或域名，會捕獲主機與目標ip與他們的子網路的封包(指定192.168.1.100，子網路192.168.1.0/24內的封包的也可以捕捉到)

* `host` ：可以指定一個ip或主機名，會捕獲主機與目標ip的封包 (指定192.168.1.100，就只包含主機與目標IP為192.168.1.100的封包，不包含子網路)

指定協議(proto)
* `tcp` ：捕獲 tcp 協議流量

* `udp` ：捕獲 udp 協議流量

* `icmp` ：捕獲 icmp 協議流量

* `ip` ：捕獲所有ip協議的流量 (tcp、udp、icmp)

* `arp` ：捕獲 arp 通信流量

封包目標或來源 (dir)
* `src`：指定封包來源

* `dst`：指定封包目標

* `src or dst`：預設，不管來源或目標


還可以用 and、or、not 來搭配過濾


# 常用指令

## 查看dns 封包
``` bash
    tcpdump -i eth0 -nt -s 500 port domain
```
## 查看特定來源的封包
``` bash
    tcpdump src host 210.27.48.1
```

## 監聽指定Port 跟 主機間的封包
``` bash
    tcpdump tcp port 9453 and host 10.0.0.2
```

# 參考資料
[使用 Tcpdump 分析查看 DNS 通信过程](https://jaminzhang.github.io/dns/use-tcpdump-to-analyze-dns-communication/)

[抓包工具tcpdump用法說明](https://iter01.com/557434.html)

[史上最簡明的 Tcpdump 入門指南，看這一篇就夠了](https://www.readfog.com/a/1687484429703942144)