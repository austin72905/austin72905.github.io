---
title: Linux 文件系統
date: 2023-7-30 02:21
categories: Linux
tags:
    - Linux
---

# 前言

Linux 中每個目錄都有其各自的作用，有些目錄也有限制訪問權限，所以安裝軟體時還是別隨心所欲地亂放，記得第一次使用時安裝nginx，竟然把它放在`/etc` 目錄下，因為些權限問題而啟動時遇到困難。今天就整理一下常見目錄的作用，以及常用操作目錄及文件的指令。

# 目錄作用

- `/boot` : 存放系統開機時必要的文件

- `/root` : root user 的家目錄，不等同於 “/”

- `/dev` : 硬體設備(keyboard、mouse)的操作文件

- `/etc` : 存放<font color=#EB5757>配置文件</font>(如:DNS、某個應用的配置檔)

- `/bin` ⇒ `/usr/bin` : 這兩個目錄都是存放常見且通用的系統命令，(如 ls（列出目錄內容）、cp（複製文件）、mv（移動文件）等)

- `/sbin`⇒ `/usr/sbin` : 存放系統管理員使用的系統級命令，(ifconfig（設置網絡接口）、fdisk（磁盤分割工具）、mount（掛載文件系統）等)

- `/opt` : (optional)用來存放第三方軟體

- `/proc` : 是一種虛擬文件系統，<font color=#EB5757>紀錄正在運行中的進程文件</font>，也能查看目前的硬體訊息如CPU，如果停止運行裡面文件就會消失，這些文件只存在於記憶體中

- `/lib` ⇒ `/usr/lib` : 所有<font color=#EB5757>C 語言的依賴包</font>都存在這裡，如使用的那些cmd 是用C 語言寫的，他的依賴包就是存在這個資料夾，可以用 strace -e open pwd  就可以看這個指令使用的依賴包有哪些

- `/tmp` : 檔案暫時存放區

- `/home` : 用戶的家目錄，裡面會有所有用戶的家目錄(除了root)

- `/var` : 系統的log 存放區

- `/run` : 它主要用於存放在系統開機期間需要在運行時創建的臨時運行時檔案（runtime files）。/run 目錄通常用於<font color=#EB5757>存放正在運行的系統和服務的狀態資訊、進程檔案、socket 檔案</font>等。這些檔案在系統重新開機後會被清理，因為 /run 目錄是一個暫存的文件系統，它存在於記憶體中，而不是永久存儲在硬碟上。裡面有PID 文件，用于存储进程的 PID（Process ID）信息

- `/mnt` : 可以用來掛載其他的文件系統

- `/media` : 可以用來<font color=#EB5757>掛載可移動裝置</font>，包括 USB 驅動器、光碟、SD 卡、移動硬碟等

- `/usr` : 存放使用者相關的應用程式和檔案，通常被稱為 "Unix System Resources" 或 "User System Resources
    * `/usr/bin` : 存放大部分使用者可執行的命令和應用程式，包括系統自帶的工具和第三方軟體。例如，ls、cp、mv 等常用命令就位於 /usr/bin 目錄下。

    * `/usr/local` : 通常被用於<font color=#EB5757>安裝本地使用者自己編譯的軟體</font>，目錄下通常包含 bin、lib、include 等子目錄，類似於 /usr 目錄的結構。

    * `/usr/share` : 共享的資料檔案，例如文件、幫助檔案、圖示。

    * `/usr/lib` : 存放系統和應用程式所使用的共享庫檔案（shared libraries）

# 檔案屬性

| Type |# of links| Owner | Group | Size | Month | Day | Time | Name |
| ---- |   ----   |  ---- | ----  | ---- | ----  | ----| ---- | ---- |
| drwxr-xr-x. | 21|root|root|4096|Feb|27|13:33|var|
| lrwxrwxrwx. | 1|root|root|7|Feb|27|13:15|bin|
| -rw-r-r-- | 1|root|root|0|Mar|27|13:15|test|

* `Type`  :  檔案的類型 與 相關權限
* `# of links` : 該檔案有幾個硬連結，如果是目錄，就是 (該目錄+其父目錄+其子目錄)
* `Owner`  :  檔案擁有者
* `Group`  :  檔案擁有群組

# 檔案類型

使用 `ls -l` 常可以看到文件屬性前面有一段英文字如`-rw-r-r--`，其中最前面的字母代表了該檔案的類型，分別為

- `d` : 目錄
- `l` : 連結
- `-` :  一般文件
- `s` : socket
- `c` : 裝置文件(keyboard)
- `p` : Name pipe
- `b` : block device


# 硬連結（Hard Link）、軟連結（Soft Link）、inode

- inode :  檔案在磁碟上的`指標`(實際上的物理位置)，每次檢索檔案時，磁碟都會用到這組數字

- 硬連結（Hard Link） :  直接連到 inode ，不經過檔案，他和檔案共享同一個數據塊，所以檔案刪除或是改名，硬連結不會被影響，不可跨文件系統，當所有硬連結和原始檔案都被刪除時，才會真正釋放檔案的磁碟空間。

- 軟連結（Soft Link） :  像是創建檔案的捷徑，透過檔案(他指向的只是檔案的路徑，而不是真實數據)，再連到 inode， <font color=#EB5757>檔案刪除或是改名，軟連結就GG了</font>，可以跨文件系統


- 指令
    * `ln` : 創建硬連結
    * `ln -s` : 創建軟連結

- 不要在同一個目錄下創建連結

使用連結時需要小心，以免因刪除原始檔案或連結檔案而丟失資料或引起檔案系統的混亂。同時，避免創建過多的`硬連結`，以免在刪除硬連結時不小心刪除了原始檔案。

# 針對文件、目錄的操作

## 創建檔案


### 創建文件 (touch)

創建單一文件 `touch 檔名`

例如: `touch test`


創建多個文件 `touch 檔名1 檔名2 檔名3`

例如: `touch test1 test2 test3`


創建多文件

`touch abc{1..9}.xyz`
會新增 abc1.xyz、abc2.xyz ... abc9.xyz 的檔案


### 創建目錄 (mkdir)

`mkdir 目錄名`

例如: `mkdir test-dir`

## 複製檔案 (cp)


### 複製文件

複製文件 `cp 檔名 新檔名`
`cp test test-bak`

### 複製目錄

-R 代表把目錄裡面的文件也都複製一份， 他是把原目錄裡面的檔案都複製一分到新目錄名下面
`cp -R 原目錄 新目錄名`

例如: `cp -R test-dir test-dir-bak`

## 尋找檔案 (find、locate)

### find

`find 查詢的起始目錄 -name "檔名"`

從當前目錄開始尋找

`find . -name "test"`

從根目錄開始尋找

`find / -name "test"`

### locate

`locate 檔名`

例如: `locate test`

### locate 、find 差異?

* `locate` 有維護一張表，他會直接去從表查，所以速度會比`find` 快很多，但是表如果還不是最新版本(該表是系統排程更新)，可能查詢結果不準確 (`updatedb`指令)

* `find` 是每次都會去文件系統查，速度較慢，但是準確

# 通配符  (WildCards)

通配符是用於匹配檔案名或目錄名的特殊字符，常用於需要批量操作一些檔名很類似， abc1、abc2、abc3...abc99，如日誌檔

可以快速搜尋符合條件的檔案，並執行相對應動作，很大的提高工作效率，常用的通配符有以下幾種

-  `*`  :  用來匹配0~ 多個字符，如想要找出所有的txt結尾的檔案， 可以用`*.txt`表示

- `?` :  匹配1個字符，如想要找出 ab1、ab2、ab3...ab9 等 ab開頭，結果只有一個字符的檔案，可以用`ab?`表示

- `[ ]` :  指定一個字符集合 例如含有x或y的檔名，可以用`*[xy]*`表示


