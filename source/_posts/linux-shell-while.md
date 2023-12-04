---
title: shell 中的 do while 語句
date: 2023-11-27 17:21
categories: Linux
tags:
    - Linux
    - shell
---

# 語法介紹

while 用於循環，根據條件執行一段代碼塊

``` bash
    # 語法
    while [ 條件 ]
    do
        # do something...
    done
```

# 範例
``` bash
    #!/bin/bash

    c = 1
    while [ $c -le 5 ]
    do
        echo welcome $c times
        # c = c+1
        ((c++))
    done
```