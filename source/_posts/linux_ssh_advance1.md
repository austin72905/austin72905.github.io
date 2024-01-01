---
title: 使用 ssh 搭配 ngrok 將遠程檔案傳到本機
date: 2024-1-1 12:00
categories: 
    - [ Linux]
tags:
    - Linux
    - ssh
---

# 前言

工作上有台生產環境的VM是用Ubuntu環境，之前上面安裝的nginx出問題，想要下載日誌到本機分析，有想過可以用ssh連線到VM，但又不想在VM 上 額外開啟ssh port，增加管理上的麻煩，後來進一步對openssh了解，才知道openssh分成客戶端與服務端，又剛好接觸到wsl

因此想到
**可以將本機 wsl 當服務端，用 ngrok 暴露對外ip，VM當客戶端，使用scp指令將檔案傳到本機的wsl**，就不用在線上環境開啟ssh port了

相關名次介紹可以參考我之前的文章
* `ssh` {% post_link linux-service-ssh Linux網路服務-ssh %}

* `scp` {% post_link linux-service-scp Linux 遠端複製-scp %}

* `wsl` {% post_link wsl-install 在win11上使用wsl2 %}


# ngrok

## 使用ngrok 開啟ssh port

本機的 ip 是不會暴露給外網的，需要透過其他的工具才能讓處在外網的VM連線到

可以使用`ngrok`， 這個工具可以把你本機的ip 暴露給外網，讓外網的人可以連線，如何安裝ngrok就不贅述

1. 在wsl宿主機上(非wsl環境)安裝ngrok

安裝完後在wsl宿主機(非wsl環境)輸入
``` bash
    # 依自己需求指定port
    ngrok tcp 22
```

接著會看到畫面出現類似

```
    tcp://6.tcp.ngrok.io:10211 -> localhost:22
```

此時外網可以透過 `6.tcp.ngrok.io` 的 `10211` port，連線到本機的 `22` port

wsl port 與 宿主機 是共用的，所以可以透過 本機的 22 port 直接連線到 wsl 22 port

# 生產環境VM

## 產生公鑰 (如果選擇輸入密碼連線則可跳過)

VM 作為客戶端

使用`ssh-keygen`建立公私鑰，

會在`~/.ssh` 新增一對公私鑰
* id_rsa
* id_rsa.pub (公鑰)







# wsl

我使用的wsl 是 ubuntu 發行版，其他發行版指令可能略有不同


## 開啟ssh 服務端

``` bash
    service ssh start

    # 或是 (依照發行版不同)

    service sshd start
```


如果還沒下載 `openssh` 服務端

``` bash
    apt install openssh-server
```

## 修改為ssh 連線時需輸入密碼 (與配置公鑰擇一)

連線時採用配置公鑰可以跳過

wsl 預設 連線時不需要輸入密碼，如果想要修改，可以到 `/etc/ssh/sshd_config` 修改配置

``` 
    PasswordAuthentication : yes
```

輸入完後，重新加載服務
`service ssh restart`


## 修改為ssh 連線使用配置公鑰 (與輸入密碼擇一)
採用連線時輸入密碼這步可以跳過

將 客戶端(也就是VM)的公鑰(`~/.ssh/id_rsa.pub`)內容貼到 wsl上的`~/.ssh/authorized_keys`，沒有此目錄跟文件，可以自行創立一個。



或者回到生產環境VM

輸入以下指令，將公鑰上傳到遠端
``` bash
    # port 跟 用戶、遠端域名 依自己的配置修改
    ssh-copy-id -p 10221 austin72905@6.tcp.ngrok.io
```




## scp 指令

回到生產環境VM

傳送檔案，假設有一個文件`test`，想要傳到遠端的`/home/austin72905`目錄

輸入
``` bash
    # port 跟 用戶、遠端域名 依自己的配置修改 (注意這裡指定port 是大寫"P")
    scp -P 10221 test austin72905@6.tcp.ngrok.io:/home/austin72905
```



就會看到文件傳送成功了

# 結語

* 如果不透過wsl 的話 ，新版本windows 其實內建已經有 openssh，但需要額外設定防火牆的樣子，也可以自行去嘗試，我自己是搞很久都沒成功，索性用wsl 比較方便。
* 使用這個方式因為使用ngrok 暴露ip，每次開啟時ip都會不一樣，每次連線VM上的 `~/.ssh/know_hosts` 都會增加一筆，要定時去刪除。

# 參考

[wsl中使用ngrok进行ssh连接](https://lostsea.cn/articles/2023/05/11/1683793910638.html)