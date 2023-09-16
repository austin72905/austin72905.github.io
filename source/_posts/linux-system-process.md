---
title: Linux 系統管理-進程相關操作
date: 2023-9-4 01:48
categories: Linux
tags:
    - Linux
---

# ps（查看各進程情況）

ps (Process Status) 是用來查看系統當前運行的進程，可以查看當前進程的狀態以及進程號等資訊，通常會先使用`ps`找出特定的進程，配合`kill`指令來終止進程。

## 進程的狀態

|狀態|定義|
| ---- |  ----  |
| R    |  運行中  (Running) |
| S    |  睡眠，等待調用  (Interruptible Sleep) |
| D    |  等待硬碟I/O  (Uninterruptible Sleep) |
| T    |  暫停狀態  (Stoped)|
| Z    |  僵屍進程  (Zombie) |
| N    |  低優先級進程 |
| <    |  高優先級進程 |
| s    |  進程的領導者|
| I    |  多線程狀態 |
| +    |  前台進程 |


## 指令風格( `-參數`、`--參數`、`參數` ) 
常會看到指令有時出現 `-`，有時 `--` ，有時前面又沒有`-`，分別代表不同風格的指令
* UNIX：`-參數`，如 `ps -ef`
* BSD：參數前沒有符號，如 `ps aux`
* GNU：`--參數`，如 `ps axu --sort -%cpu`

## 常用參數
* <font color=#EB5757>`a`</font>：顯示當前終端運行的進程
* <font color=#EB5757>`u`</font>：顯示用戶相關進程與用戶相關屬性，格式會從 
    `UID PID PPIC C STIME TTY TIME CMD` 轉變成  `USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND` 的格式
* <font color=#EB5757>`x`</font>：顯示沒有控制終端的進程
* <font color=#EB5757>`j`</font>：顯示與作業有關的信息（顯示的列）：會話期ID（SID），進程組ID（PGID），控制終端（TT），終端進程組ID（TRGID）
* <font color=#EB5757>`f`</font>：樹狀結構，表達程序間的相互關係


* <font color=#EB5757>`-e`</font>：顯示所有的進程
* <font color=#EB5757>`-f`</font>：列出進程的完整資訊(最詳盡)
* <font color=#EB5757>`-u`</font>：列出指定用戶運行的進程

* <font color=#EB5757>`--sort +-列名`</font>：依照哪個列的數值排序，`+` 為升序(小 ~ 大) ; `-`為降序(大 ~ 小)


## `ps -f`列出的詳細資訊

以下是 `ps -f` 命令列出的進程資訊的詳細說明：

| 名稱    |  定義 |
| ----    |  ----  |
| PID     |  進程 ID，是系統用來識別進程的唯一編號 |
| USER    |  進程所屬的使用者 |
| STAT    |  進程的狀態 |
| TTY     |  進程連接的終端，若與終端無關顯示 `?`|
| TIME    |  進程使用 CPU 的時間 |
| START   |  進程啟動的時間 |
| %CPU    |  進程使用的 CPU 使用率 |
| %MEM    |  進程使用的記憶體使用率 |
| VSZ     |  進程使用的虛擬記憶體(內存)大小 |
| RSS     |  進程使用的實際記憶體(內存)大小 |
| COMMAND |  進程的命令行 |

## 範例

* 不加任何參數查看當前使用的終端會話(你當前使用的帳號)正在使用的進程
```bash
    ps
```

* 顯示所有進程的完整資訊 (最常用)
```bash
    ps -ef
```
* 想要查看CPU、記憶體等資訊時 (u 跟 j 只能選一個用)
```bash
    ps aux
```

* 查看與job 作業有關的列資訊，也可以用來查看守護進程 (u 跟 j 只能選一個用)
```bash
    ps axj
```

* 查詢當前nginx 相關的進程，通常可以用在當nginx 占用進程號，需要將他停止時
```bash
    ps -ef | grep nginx
```

* 後續使用grep 又想保留標題行
```bash
    ps -ef | grep -E "^USER|nginx"

    # 看你選擇要顯示的列擇一
    ps -ef | grep -E "^UID|nginx"
```


* 查看root用戶運行的進程
```bash
    ps -f -u root
```

* 依照cpu 使用率排序 大 ~ 小
```bash
    ps aux --sort -%cpu
```




# kill（中止進程）

* 用來終止特定進程的運行，通過向進程發起信號，來結束相對應的進程
* 需要有足夠的權限，才能夠停止其他用戶的進程
* 若不加任何參數，預設為`-15`，使用信號15，進程會正常退出

常用參數
* -l：列出所有的信號名稱
* -u：終止指定用戶的進程

## 常用信號
```bash
    kill   PID (預設使用-15停止進程)

    kill -1  PID (重新開啟進程)

    kill -2  PID (像是crtl+c 一樣的停止進程)

    kill -9  PID (強制停止進程)

    kill -15  PID (發送信號給進程，請他正常退出)
```

## 查看所有的信號名稱
```bash
    kill -l
```

## 強制退出進程
```bash
    kill -9 PID
```

## 終止指定用戶的進程
```bash
    kill -u 用戶帳號
```

# nice（設定進程的優先級）

設定進程的優先級，設定數值為-20~19，數字越少，進程優先級越高

```bash
    nice -n -10 sleep 500
```
* 這會將 `sleep 500` 設置為較高的優先級（-10），使其在系統資源分配中更有可能獲得更多的CPU時間。

* nice 命令通常需要超級用戶權限（root）才能將進程優先級降低。普通用戶只能將其提高到0



# 切換進程的指令

## ctrl+z 、 ctrl+c

* `ctrl+c`：強制中斷進程，只能中斷前台運行的程序，如果要中斷後台運行的程序要用到`kill`指令

* `ctrl+z`：暫時停止進程 (掛起)

## jobs（要查看當前終端會話中的所有作業及其對應的作業號）

要查看當前終端會話中的所有作業及其對應的作業號，可以使用 `jobs` 命令。這將列出所有正在運行的作業及其狀態。

輸出類似如下
```
    # 分別為 作業號  運行狀態   程序名稱
    [1] Stopped             sleep 500
    [2] Running             tcpdump 
```

## bg（將進程抓到後台去執行）

將進程抓到後台去執行

* 將上一個掛起的進程，拿到後台執行
```bash
    bg
```



* 將指定作業號的進程，拿到後台執行
```bash
    bg 作業號
```

運行完會看到類似輸出
```
    # 分別為 作業號  PID
    [1] 4327

```

## fg（將進程抓到前台終端去執行）

將進程抓到前台終端去執行

* 將上一個掛起的進程，拿到前台執行
```bash
    fg
```

* 將指定作業號的進程，拿到前台執行
```bash
    fg %作業號
```

## nohup（讓一個命令在後台運行，且不受終端關閉影響）

讓一個命令在後台運行，且不受終端關閉影響

雖然 nohup 可以讓一個進程在後台持續運行，但它並不會自動將該進程轉化為典型的守護進程。它仍然與終端會話相關聯，只是在終端關閉後也繼續運行。

運行時預設會創建一個`nohup.out`文件紀錄指令運行的過程

```bash
    nohup 指令 &
```

* 輸出到指定文件
```bash
    nohup sleep 500 > custom.out &
```
* 將標準錯誤輸出到指定文件
```bash
    nohup sleep 500 > custom.out 2>&1 &
```
<font color=#EB5757>`2>&1`</font> ，2 為標準錯誤，1為標準輸出，也就是將標準錯誤的輸出，覆蓋掉原本標準輸出的位置`custom.out`

* 忽略標準輸出 跟 標準錯誤 的訊息
```bash
    nohup sleep 500 > /dev/null  2>&1 &
```
<font color=#EB5757>`/dev/null`</font> ，是一個特殊設備文件，他將一切寫入他的輸入給丟棄

    <font color=#EB5757>`2>&1`</font>， 把標準錯誤輸出也一起丟到標準輸入中，也就是把標準錯誤也一起丟到 <font color=#EB5757>`/dev/null`</font>



## 範例介紹-使用sleep 指令練習 進程切換

1. 先在前台運行sleep 指令

    `sleep 500`，這個指令會占用終端指定秒數(500)後，再將終端返回給用戶，適合拿來練習操作掛起、後台執行等操作

2. 將它掛起 <font color=#EB5757>`ctrl+Z`</font>

    此時 `sleep 500` 這個進程暫時停止了

3. 使用 `jobs` 指令查看當前的任務有哪些，可以看到

    ```bash
        # 以下資訊分別為
        # 作業號 ,  進程狀態  , 運行的程式名稱

        [1]+ Stopped     sleep 500
    ```

    或是想要使用 `ps -ef | grep sleep` 檢查進程是否還在

4. 若想讓 `sleep 500` 在後台執行，輸入 <font color=#EB5757>`bg 作業號`</font>，也就是 <font color=#EB5757>`bg 1`</font> ，或是使用<font color=#EB5757>`bg`</font> 重新讓上一次掛起的進程再度開始在後台執行，運行完會出現類似 作業號,  PID 的輸出

    ```bash
    # 作業號  PID
    [1]  4237
    ```

5. 如果一開始執行就想要讓他在後台執行可以使用 <font color=#EB5757>`指令 &`</font> ，也就是 `sleep 500 &` 


6. 若想將 `sleep 500` 回到前台執行 使用 <font color=#EB5757>`fg %作業號`</font> ，也就是 <font color=#EB5757>`fg %1`</font>


7. 最後不想執行了，使用 <font color=#EB5757>`ctrl+c`</font> 停止

# 參考
[每天一个linux命令（42）：kill命令](https://www.cnblogs.com/peida/archive/2012/12/20/2825837.html)

[Linux-ps命令学习](https://www.jianshu.com/p/943b90150c10)

[Linux ps 命令](https://www.runoob.com/linux/linux-comm-ps.html)

[Linux 使用 grep 保留表头（首行|标题）的解决方案](https://blog.csdn.net/welcomword/article/details/122000607)

[bg和fg指令（整理）以及 Linux中Ctrl+C、Ctrl+D等按键操作&进程相关命令](https://blog.csdn.net/deniece1/article/details/102770363)