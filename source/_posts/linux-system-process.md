---
title: Linux 系統管理-進程相關操作
date: 2023-9-4 01:48
categories: Linux
tags:
    - Linux
---

# 進程相關指令

## ps（查看各進程情況）
```bash
    ps -ef
```
```bash
    ps axj
```
* a: 顯示所有
* x：顯示沒有控制終端的進程
* j：顯示與作業有關的信息（顯示的列）：會話期ID（SID），進程組ID（PGID），控制終端（TT），終端進程組ID（TRGID）


## kill（中止進程）

用來終止特定進程的運行

通過向進程發起信號，來結束相對應的進程

kill -l

kill -1 PID
kill -9 PID




需要有足夠的權限，才能夠停止其他用戶的進程

停止進程

若不加任何參數，預設為-15，使用信號15，進程正常退出

常用參數
* -l：列出所有的信號名稱
* -u：終止指定用戶的進程

### 常用

kill   PID (預設使用-15停止進程)

kill -1  PID (重新開啟進程)

kill -2  PID (像是crtl+c 一樣的停止進程)

kill -9  PID (強制停止進程)

kill -15  PID (發送信號給進程，請他正常退出)

## 查看所有的信號名稱
```bash
    kill -l
```

### 強制退出進程
```bash
    kill -9 PID
```

### 終止指定用戶的進程
```bash
    kill -u 用戶帳號
```

## nice（設定進程的優先級）

設定進程的優先級，設定數值為-20~19，數字越少，進程優先級越高

```bash
    nice -n -10 sleep 500
```
* 這會將 `sleep 500` 設置為較高的優先級（-10），使其在系統資源分配中更有可能獲得更多的CPU時間。

* nice 命令通常需要超級用戶權限（root）才能將進程優先級降低。普通用戶只能將其提高到0

# 參考
[每天一个linux命令（42）：kill命令](https://www.cnblogs.com/peida/archive/2012/12/20/2825837.html)