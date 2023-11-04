---
title: Linux alias - 為你的指令取別名，告別冗長的指令輸入
date: 2023-11-04 15:15
categories: Linux
tags:
    - Linux
---

# 前言
在linux 系統中常常需要輸入命令行，有時甚至要組合多個命令，每次都要輸入冗長的指令就讓人心煩。
linux 中有個`alias`命令，能夠自定義命令的別名，這些別名可以是<font color=#EB5757>`縮寫`</font>或是更簡短的指令，這樣可以幫助我們更快速的輸入指令，提高工作效率

# 語法格式

```bash
    alias 別名='指令'
```

* 這些指令可以是系統的指令，也可以是執行某個自己寫的腳本

* 也可以搭配輸入參數使用 ，`$1`、`$2` 會帶入第1個跟第2個輸入的參數

範例
```bash
    alias test='echo hi $1'
```

此時輸入
```bash
    test austin
```
就會輸出
```
    hi austin
```

# 查看別名

可以看到目前定義的命令別名有哪些

```bash
    alias
```


# 修改別名

## 暫時修改

這樣的修改方式，在關閉終端後，下次登陸時，這些別名都不在了

### 建立別名
```bash
    alias 別名='指令'
```

### 取消別名
```bash
    unalias 別名
```

## 永久修改

如果想要永久修改可以，需要使用配置檔去配置

### 針對單一用戶更改

`/home/username .bashrc`

### 針對所有用戶修改

`/etc/bashrc`

## 使用範例

以建立自己能夠使用的別名 為例

1. 使用vi 開啟 `~/.bashrc`  (打開自己家目錄的`.bashrc`)

```bash
    vi ~/.bashrc
```

2. 在檔案的最下面輸入

```bash
    alias yourcommand='script command'

    # 範例
    alias showlog='bash /usr/local/yourscript.sh'
```
3. 退出文件後，重新刷新終端

```bash
    source ~/.bashrc
```

# 參考資料
[[Linux] 使用 alias 自定義指令](https://www.ltsplus.com/linux/linux-command-alias)

[Linux 自訂指令 Alias 別名](https://clay-atlas.com/blog/2020/01/05/linux-chinese-tutorial-ubuntu-alias/)