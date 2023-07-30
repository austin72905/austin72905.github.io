---
title: 安裝golang (ubuntu 20.04)
date: 2023-7-21 16:43
categories: golang
tags:
    - golang
---

## 安裝步驟

1. 到官網連結安裝，(官網連結可以能會修改)，安裝的版本選 amd64

```bash
    wget https://go.dev/dl/go1.20.6.linux-amd64.tar.gz
```

2. 解壓縮到 `/usr/local`，解壓縮完會發現  `/usr/local`` 多了一個go 目錄

```bash
    # -C 解壓縮的目標目錄
    sudo tar -C /usr/local -xzf go1.20.6.linux-amd64.tar.gz
```

3. 在 `/etc/profile` 檔案 新增 go 的 環境變數

```bash
    # 編輯 /etc/profile
    sudo vi /etc/profile

    # 在檔案中新增這行
    export PATH=$PATH:/usr/local/go/bin 
```

4. 刷新當前shell環境，獲取到/etc/profile最新的配置

```bash
    source /etc/profile
```

5. 也可以修改~/.profile

```bash
    vi ~/.profile

    # 在檔案中新增這行
    export PATH=$PATH:/usr/local/go/bin 

    # 刷新當前shell環境
    source ~/.profile
```

6. 檢查下是否安裝成功
```bash
    go version

    # 應該會出現
    go version go1.20.6 linux/amd64
```

關掉 cmd 後可能會出現”無法找到go 指令”，可以重啟VM 試試

## 參考
[[Golang] 安裝 (Windows & ubuntu20.04)](https://quietbo.com/2022/07/21/golang-%E5%AE%89%E8%A3%9D-windows-ubuntu20-04/)

[Linux中source命令的使用方式](https://www.linuxprobe.com/linux-source-useful.html)