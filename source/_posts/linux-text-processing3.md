---
title: Linux 文本處理(3) (cut、grep、awk、sort、uniq、wc)
date: 2023-8-28 00:53
categories: Linux
tags:
    - Linux
    - 文本處理
---

# 前言

在前面兩篇關於文本處理的文章
{% post_link linux-text-processing Linux 文本處理(1) (查看文件內容) %}


以及
{% post_link linux-text-processing2 Linux 文本處理(2) (寫入資料到文件) %}


介紹了基本的文本操作，今日就來介紹其他較為進階的文本操作指令，包含搜尋關鍵字、排序、切割文本等操作。

# cut (切割、擷取文字)

常用來切割文字，擷取特定位置的字符，或是以特定的分割符，擷取特定的欄位，很像一般程式語言中的 **<font color=#EB5757>`split()`</font>** 方法，會以每`行`作為單位執行

常用參數

* `-c`：擷取特定位置的字符(從1開始)，擷取多個位置用 `,`分開，連續位置用 `-`
* `-f`：用分割符分割後，擷取哪列的值，多列也是用`,`分隔，都配合 `-d` 使用，沒有設定分隔符就是單純把每行印出來
* `-d`：指定分割符

以下為使用範例

## 擷取特定位置字符
*  **切割文本文件每行的第1至3个字符：**
    
    ```bash
        cut -c 1-3 文件名.txt
        
        # 假設文本內容為
        abcddd
        bbcddd
        
        # 輸出會為
        abc
        bbc
    ```
    
*  **擷取文件內每行的1,3,5 的字符**
    
    ```bash
        cut -c 1,3,5 文件名.txt
        
        # 假設文本內容為
        abcddd
        bbcddd
        
        # 輸出會為
        acd
        bcd
    ```
    
* **擷取文件內每行1至3、6至9 的字符**
    
    ```bash
        cut -c 1-3,6-9 文件名.txt
        
        # 假設文本內容為
        abcdddasdg
        bbcdddasdg
        
        # 輸出會為
        abcdas
        bbcdas
    ```

## 以分隔符分割，擷取欄位值    
* **切割以逗號分隔的CSV文件的第2列和第4列：**
    
    ```bash
        cut -d ',' -f 2,4 數據.csv
    ```
    
* **切割以冒号分隔的密碼文件的用户名字段：**
    
    ```bash
        cut -d ':' -f 1 /etc/passwd
    ```

## 與其他命令結合使用    
* **從標準輸入讀取數據，切割第1、第3和第5个字符：**
    
    ```bash
        echo "Hello,World" | cut -c 1,3,5
    ```

# grep (搜尋關鍵字)

`grep`、 `sed`、`awk` 並稱linux 文本處理三劍客

`grep`是用來`搜尋關鍵字`，它可以使用正規表示式搜尋文字，並把匹配的行打印出來。全稱是 Global Regular Expression Print。

常用參數

* **<font color=#EB5757>`-i`</font>** ：忽略大小寫進行搜索

* `-r` 或 `-R` ：遞歸地在`目錄`中搜索匹配的文本

* `-l` ：顯示包含匹配文本的`文件名`。通常配合通配符`*`使用

* `-n` ：顯示匹配文本所在的行號

* `-v` ：反轉搜索，只顯示不匹配模式的行

* **<font color=#EB5757>`-c`</font>** ：顯示匹配行的數量

* `-A num` ：顯示匹配行及`後面` num 行的內容

* `-B num` ：顯示匹配行及`前面` num 行的內容

* **<font color=#EB5757>`-C num`</font>** ：顯示匹配行以及`前後`各 num 行的內容
* `--color=always` ：關鍵字高亮


常用範例

## 搜尋關鍵字
* **在文件 "file.txt" 中搜索包含 "apple" 的行：**
    
    ```bash
        grep "apple" file.txt
    ```
## 針對目錄遞歸搜尋(-r)
* **在目錄 "/home" 及其子目錄中搜索包含 "error" 的文本文件：**
    
    ```bash
        grep -r "error" /home
    ```

## 忽略大小寫(-i)   
* **忽略大小寫，在文件 "file.txt" 中搜索 "Code: 502"：**
    
    ```bash
        grep -i "Code: 502" file.txt
    ```

## 顯示行號(-n)
* **在文件 "file.txt" 中搜索 "apple" 並顯示行號：**
    
    ```bash
        grep -n "apple" file.txt
    ```

## 顯示包含匹配文本的文件名(-l)    
* **在當前目錄搜尋顯示包含 "apple" 的文件名，而不顯示匹配的行：**

    ```bash
        grep -l "apple" *
    ```

* **在指定目錄還有其子目錄，搜尋內容包含"test"的文件名：**

    ```bash
        grep -r "test" -l /home/
    ```

* **搜尋指定目錄下的文件，內容包含"test"的文件名：**

    ```bash
        grep  "test" -l /home/*
    ```
## 顯示匹配行及前後num行(-C數字)
* **搜尋關鍵字，顯示匹配行及其後4行**

    ```bash

        grep -C4 "status: 502" 2020-1-1.log 
    ```

## 關鍵字高亮 `--color=always`

* 關鍵字高亮，如果有用到管道，需要兩個都加
    ```bash
        grep -C4 "status: 502" 2020-1-1.log  --color=always | grep -C4 "url"  --color=always
    ```


# egrep (grep高配版)

grep 的高級版，可以同時搜尋多個關鍵字

    ```bash
        egrep "A|B" 文件名.txt
    ```

# awk (以分隔符擷取文字)

簡單來說就是逐行讀取文本，以 `空格 or Tab` 將每行分割成小單位，也可以自訂分隔符號，最後將結果輸出

## 語法格式
```bash
    awk '{動作}' 文件名

    awk '條件 {動作}' 文件名
```

* 其中 `{動作}` 最常使用的是 print，還有其他如printf (格式化輸出)、if 語句，操作一些內置的函數如 length() 獲取字串的長度、substr() 擷取字串等

* 如果指定`條件`，當條件符合時才會執行動作

常用參數

* `-F`：指定分隔符，默認為 <font color=#EB5757>`空格 or Tab`</font>
* `-f`：使用awk腳本，來處理文本
* `-v`：awk需要使用shell 裡面的變數時，需要先用-v 定義，無法直接使用

常用的變數
* `$0`： 打印整行 ，之後的數字代表字符以分隔符分隔後所在的位置 $1、$2...以此類推
* `NR`： 每行的行號
* `NF`： 每行用空格分隔後，字段的數量

## 內置變數

前面都不需要加$

* `FILENAME`： 當前文件名
* `FS`：       分隔符，默認為 空格 or Tab
* `RS`：       行分隔符，默認為  換行符
* `OFS`：      輸出的字段的分隔符，默認為空格，可以用 OFS="~" 來指定，前面加上`-v`
* `ORS`：      輸出行的分隔符，默認為空格
* `OFMT`：     輸出的數字的格式，默認為%.6g
* <font color=#EB5757>`NF`</font>：       每行用空格分隔後，字段的數量， 所以也可以用此找出每行的最後一個字

## 內置函數

* `toupper()`：大寫
* `tolower()`：小寫
* `length()`： 長度
* `substr()`： 擷取字串
* `sin()`：    正弦
* `cos()`：    余弦
* `sqrt()`：   平方根
* `rand()`：   隨機數

## 常用範例

### 基本用法

有一文本`test.txt`，內容為`this is a test`

* 打印出整行
```bash
    awk '{print $0}'
```
    `Output:`
```
    this is a test
```

* 打印出每行 以空格分隔後的第一個單詞
```bash
    awk '{print $1}'
```
    `Output:`
```
    this
```

* 將分隔符設定為 : 並打印第二個字
```bash
    awk -F ':' '{print $2}'
```
    `Output:`
```
    is a
```

* 使用某個awk腳本處理文件
```bash
    awk -f {awk脚本} {文件名}
```


* 使用shell 裏面的變數
```bash
    count=100
    awk -v cc=$count {print cc}
```
    `Output:`
```
    100
```

* 設定輸入自段的分隔符(OFS)，加上 `-v`
```bash
    
    awk -v OFS='\n' -F '-' '{print $2}'
```


### 設定條件

有一文本 `awk_test_file`，內容如下
```
    2 this is a test
    3 Are you like awk
    This's a test
    10 There are orange,apple,mongo
```

* 打印出 不包含 is 的行
```bash
    awk '$0 !~ /is/' awk_test_file
```
    `Output:`
```
    3 Are you like awk
    10 There are orange,apple,mongo
```

* 打印出奇數行
```bash
    awk 'NR % 2 == 1 {print $0}' awk_test_file
```
    `Output:`
```
    2 this is a test
    This's a test
```

* 除了打印變數，如果想要打印一些字符用””包起來
```bash
    awk  '{print NR ") " $0}' awk_test_file
```
    `Output:`
```
    1) 2 this is a test
    2) 3 Are you like awk
    3) This's a test
    4) 10 There are orange,apple,mongo
```


### if 語句
比較複雜的情況才需要用，不然用一般的條件判斷就行了

if 需要寫在 `{動作}`裡面

else 前記得要分號`;`

* 打印奇數行，其他就打印x
```bash
    awk '{if(NR % 2 == 1) print $0; else print "x"}' awk_test_file
```
    `Output:`
```
    2 this is a test
    x
    This's a test
    x
```

### 內置函數用法

* substr(字符串, 起始位置, 长度)
```bash
    echo "Hello, World" | awk '{ print substr($0, 1, 5) }'
```
    `Output:`
```
    Hello
```


# sort (排序)

用於對文本文件中的內容進行 **<font color=#EB5757>`排序`</font>**，按照字母順序or數字順序，以每`行`作為單位執行

若未指定文件名，`sort` 命令將會從標準輸入中讀取資料。

常用參數

* `-r`：倒序排序
* `-f`：忽略大小排序
* `-k`：根據指定的欄位進行排序(每個欄位預設以`\t(空格)`分割)。例如，`k 2` 表示按照第二個欄位進行排序。
* `-n`：按照數字大小進行排序，而不是用英文
* `-t`：指定欄位分隔符
* `-u`：重複的行只保留一行

以下是常用範例

## 排序
* **對文本文件進行字母順序排序：**
    
    ```bash
        sort 文件名.txt
    ```

## 以數字進行排序(-n)    
* **對包含數字的檔案進行數值排序：**
    
    ```bash
        sort -n 文件名.txt # 如果沒有數字就預設使用英文字母排序
    ```
## 指定分隔符，選擇特定欄位進行排序(-t)    
* **對包含 CSV 資料的檔案，依照第二個欄位的數值進行排序：**
    
    ```bash
        sort -t ',' -k 2 資料.csv
    ```

## 倒序(-r)    
* **對文本文件進行倒序排序並移除重複的行：**

    ```bash
        sort -r  文件名.txt
    ```

## 去重複(-u)
* **對文本文件進型排序並移除重複的行：**

    ```bash
        sort  -u 文件名.txt
    ```

# uniq (去重複)

主要用於處理已經排序過的文本文件，能夠`去除重複`的行或報告重複行的數量。

常用參數
* `-c`：除了顯示去重複後的數據外，也會在每行顯示該行文字出現的數量
* `-d`：只顯示重複行 (exclude 不重複行)
* `-i`：去重複時，忽略大小寫

常用範例

## 去重複
* **去除已排序文件中連續重複的行：**
    
    ```bash
        uniq 文件名.txt
    ```

## 顯示重複數量(-c)  
* **顯示已排序文件中每行重複的數量：**
    
    ```bash
        uniq -c 文件名.txt
    ```
## 僅顯示重複行(-d)    
* **僅顯示已排序文件中重複的行，去除不重複的行：**
    
    ```bash
        uniq -d 文件名.txt
    ```
## 忽略大小寫(-i)    
* **忽略大小寫，去除已排序文件中連續重複的行：**
    
    ```bash
        uniq -i 文件名.txt
    ```
## 搭配sort指令使用
* **搭配sort 指令使用**

    ```bash
        sort 文件名.txt | uniq -c
    ```
* **搭配sort、awk 指令使用，打印出兩個文件不一樣的部分**

    ```bash
        #!/bin/bash
      
        old=`tr ',' '\n' < o_content | awk -v OFS='\n' -F '-' '{print $2}'`

        new=`tr ',' '\n' < n_content | awk -v OFS='\n' -F '-' '{print $2}'`


        # 執行去重複, 只打印出 不重複的部分
        # "" 包注可以保留制表符 \n \t 那些
        echo "$old$new" | sort  | uniq -c | awk '$1 == 1  {print $0}'

    ```

# wc (計數)

用來計算文本文件內各項數值的數量
沒有輸入任何參數，預設輸出該文件有 幾行、幾個單字、幾個byte

常用參數

* `-l`：有幾`行`，通常日誌每個欄位都用行區分，所以這個是最泛用的
* `-w`：有幾個`單字`，hello world 算2個單字
* `-c`：有幾個`byte`
* `-m`：有幾個`char`

常用範例

## 顯示幾行(-l)
* **該文件有幾行**

    ```bash
        wc -l 文件名.txt
    ```
## 搭配其他指令，計算筆數
* **搭配其他指令找到的資料，計算有幾筆**

    ```bash
        grep "status: 502" 文件名.txt | wc -l
    ```

# tr (取代文字)

可以取代一個文本內的文字

## 語法格式
```bash
    tr '要取代的文字' '取代成的文字'
```

## 常用範例

* 將文本內的`,`替換成`\n`

```bash

    # 將 inputfile.txt 內的 , 替換成 \n ，寫入到 outfile.txt
    tr ',' '\n' < inputfile.txt > outfile.txt

    # 使用管道

    echo "helo,caal,sail,bill" | tr ',' '\n'

```

# 參考資料
[Linux 文本处理三剑客：grep、sed 和 awk](https://zhuanlan.zhihu.com/p/110983126)

[awk 入门教程](https://www.ruanyifeng.com/blog/2018/11/awk.html)