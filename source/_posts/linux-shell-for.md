---
title: shell 中的 for 語句
date: 2023-11-27 16:36
categories: Linux
tags:
    - Linux
    - shell
---

# 語法介紹

for 可以用來遍歷數組，而這個數組很泛用，可以是數字、文件列表或是命令執行的結果

``` bash
    #!/bin/bash

    for i in 1 2 3 4 5
    do
        echo $i
    done


    for i in a b c d e
    do
        echo $i
    done

    # 想要用一般for迴圈時可以用
    for i in (1..5)
    do
        echo $i
    done

```

# 常用範例

* 遍歷命令執行的結果
``` bash
    for user in $(ls /home)
    do
        echo "User: $user"
    done
```

* 遍歷文件列表
``` bash
    # 遍歷當前目錄下 後綴為.txt的文件
    for file in *.txt
    do
        echo "Processing file: $file"
    done
```

* 遍歷數組
``` bash
    fruits=("apple" "banana" "cherry")
    # [@] 需要這樣寫才能遍歷整個數組
    for fruit in "${fruits[@]}"
    do
        echo "Fruit: $fruit"
    done
```