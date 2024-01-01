---
title: Linux網路服務-ssh
date: 2023-12-21 11:34
categories: Linux
tags:
    - Linux
    - ssh
---

# 前言

ssh (Secure Shell)用於在計算機之間進行遠程通信的協議，與早期使用的telnet 以明文方式傳遞數據不同，ssh 在遠程通信時，傳輸的數據是經過加密的，有效防止信息外洩。


# 使用方式

ssh 協議有多個實現，像是windows 上常用的putty，或是非常多人使用的`OpenSSH`，今天介紹的就是`OpenSSH`的使用與配置方式

`OpenSSH`分為了客戶端/服務端，客戶端就是你本機，服務端就是你要連線的遠程主機，在 Linux 發行版都預設安裝了客戶端，服務端才需要另外下載

## 查看是否安裝過
``` bash
    # 可以先用此指令檢查ssh 服務端是否已經安裝 
    service ssh status

    # 或是 (依照發行版不同)

    service sshd status
```

## 安裝
``` bash
    # 安裝服務端
    apt install openssh-server

    # 安裝客戶端
    apt install openssh-client
```

## 啟動 ssh 服務端
``` bash 
    service ssh start

    # 或是 (依照發行版不同)

    service sshd start
```

## 連線遠端

### 連線語法
``` bash
    ssh 用戶名@遠端服務器ip

    # 範例
    ssh austin72905@192.168.68.127
```

* `遠端服務器ip` 可以是 <font color=#EB5757>`ip/域名/別名`</font>



### 參數
* `-p`：指定連線的端口號，默認為`22`
* `-i`：指定連線使用的私鑰文件，當使用 `ssh-keygen` 生成公私鑰時不是使用默認的文件名稱才需要指定



第一次連線時會出現  <font color=#EB5757>`(EDCSA )can’t be establish`</font> 字樣，之後會要你輸入密碼

## 退出
在連線到遠端的情況下，在終端輸入`exit` 就行

## 服務端ssh 配置文件

在 `/etc/ssh/sshd_config`，可以修改一些連線配置，如端口號(預設為22)

# 進階配置

客戶端當下載完之後都會自動生成一個`~/.ssh` 目錄

裡面的配置文件有

## 客戶端

| 配置文件 | 作用 |
| ---- |   ----   |
| `known_hosts` | 曾經連過的服務器，客戶端第一次連接服務端時會有個授權提示，確認後會記錄在這個文件，下一次登陸就不需要授權提示了 |
| `config` | 可以配置一些連線到服務端的設定，例如：連線`別名` |

## 服務端

| 配置文件 | 作用 |
| ---- |   ----   |
| `authorized_keys` | 客戶端免密碼連線使用的公鑰 |

## 設置別名

每次連線都要打用戶、ip 、port很麻煩，可以在 `~/.ssh/config` 裡面設置別名，免去每次都要輸入的麻煩。


需要按照以下格式書寫，只有 <font color=#EB5757>`HostName`</font> 是必選項，其他都可以省略

``` yml
    Host hat
        HostName 192.168.68.127
        User austin72905
        Port 22
```

配置完之後只要輸入
``` bash
    ssh hat
```

等價於你輸入
``` bash
    ssh austin72905@192.168.68.127
```

## 免輸入密碼（配置公私鑰）

這個東西不只`ssh` 連線時使用到，`github` 自從推行免密碼發佈後，也是用這個方式


### 生成公私鑰

生成一對公私鑰，私鑰自己保存，公鑰上傳到服務端，輸入
``` bash
    ssh-keygen
```

輸入後一路按`enter`

默認會在 `~/.ssh` 目錄下新增兩個文件，windows 會在使用者目錄內 的 `.ssh` 目錄 新增

* <font color=#EB5757>`id_rsa`</font>  (私鑰)
* <font color=#EB5757>`id_rsa.pub`</font> (公鑰)

也可以輸入一些參數做額外設定

* `-t`：加密方式(默認rsa)
* `-f`：密鑰文件名  (如果自訂義文件名，使用ssh 時要用 `-i` 指定密鑰文件，默認則可以省略)
* `-c`：註解，會附加在密鑰尾部


### 設定私鑰的權限 (可選)
可以設定只有自己可讀 ，輸入 <font color=#EB5757>`chmod 400 id_rsa`</font>，避免被修改到

### 上傳公鑰到服務端
``` bash
    ssh-copy-id 用戶@服務器ip
```

* 上傳之後服務端會出現一個 `~/.ssh/authorized_keys`


###  windows 無法使用 `ssh-copy-id`

如果是windows 會無法使用`ssh-copy-id`這個指令，
只要把本機的 <font color=#EB5757>`公鑰`</font> (`id_rsa.pub`)複製到服務端上的 <font color=#EB5757>`~/.ssh/authorized_keys`</font> 文件就可以了，這也是`ssh-copy-id`腳本實際上做的事。

公鑰格式大概會是 `ssh-rsa AAA.....5js= user@MSI`。

如果該文件有多個公鑰，就用換行符分開。

之後`ssh`就可以不用輸入密碼，直接連線遠端了

## 如何將密鑰也寫入config?

前面提過，如果是自訂密鑰文件的名稱，之後都需要使用 `-i` 指定密鑰文件連線，每次都要輸入很麻煩，其實他也是可以寫入`config` 文件中

使用 <font color=#EB5757>`IdentityFile`</font> 參數配置

``` yml
    Host hat
        HostName 192.168.68.127
        User austin72905
        Port 22
        IdentityFile ~/.ssh/keyfile
```

# 參考
[Ssh小白入门教程一次弄懂ssh入门到精通](https://a-nomad.com/ssh)