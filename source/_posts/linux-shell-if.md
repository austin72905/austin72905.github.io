---
title: shell 中的 if 語句
date: 2023-11-27 09:39
categories: Linux
tags:
    - Linux
    - shell
---

# 語法介紹
``` bash
    if [ 條件 ]
    then
        動作
    else
        動作
    fi
```
* [  ]兩邊都需要空一格
* 變數與判斷式都需要空一格

## 變數與判斷式都需要空一格
``` bash
    #!/bin/bash

    a="sun"

    if [ $a = "mon" ]
    then
        echo "cool"
    else
        echo "no"
    fi
```

# 條件判斷

## 字串判斷
可以使用 <、>、= (bash 中 `=` 、`==` 是等價的，建議用 `=`)

## 數值判斷
* `-gt`：大於
* `-ge`：大於等於
* `-lt`：小於
* `-le`：小於等於
* `-eq`：等於
* `-ne`：不等於

# [] 跟 [[]] 差異?


* `[]` 通常用於執行基本的條件測試，使用時，需要注意變數是否包含空格，以避免錯誤

* <font color=#EB5757>`[[]]`</font> 是Bash引入的擴展條件測試構造
* <font color=#EB5757>`[[]]`</font> 可以進行更複雜的條件測試，例如正規表達式匹配

使用 <font color=#EB5757>`[[]]`</font> 比較好，不會因為包含空格而出錯，但是bash以外有些舊版的shell 可能不支援

# 常用範例

## 檢查某個文件是否存在
``` bash
    #!/bin/bash

    # -e 代表檢查某個文件是否存在

    clear
    if [ -e ./error.txt ]
    then
        echo "file exist"
    else
        echo "file noexist"
    fi
```
