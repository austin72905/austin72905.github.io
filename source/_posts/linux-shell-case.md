---
title: shell 中的 case 語句 (switch)
date: 2023-11-27 22:59
categories: Linux
tags:
    - Linux
    - shell
---

# 語法介紹

有點像是其他語言的switch

``` bash
    case expression in
        pattern1)
            匹配 pattern1，就會執行語句
            ;;
        pattern2)
            匹配 pattern2，就會執行語句
            ;;
        pattern3)
            匹配 pattern3，就會執行語句
            ;;
        ……
        *)
            default，預設都不匹配所有pattern時，預設執行的語句
    esac
```

* 每個pattern後面接 `)` ， 只要符合該pattern，就會執行後面對應的語句
* `;;` 相當於 break
* `*` 相當於 default

* expression 可以是一個變數，或是某個指令執行的結果
* pattern 可以是數字、字串，也可以使用正則表達式

# 範例

``` bash
    #!/bin/bash
    printf "Input integer number: "
    read num
    case $num in
        1)
            echo "Monday"
            ;;
        2)
            echo "Tuesday"
            ;;
        3)
            echo "Wednesday"
            ;;
        4)
            echo "Thursday"
            ;;
        5)
            echo "Friday"
            ;;
        6)
            echo "Saturday"
            ;;
        7)
            echo "Sunday"
            ;;
        *)
            echo "error"
    esac
```

## 配合正則表達式

``` bash
   #!/bin/bash
    printf "Input a character: "
    read -n 1 char
    case $char in
        [a-zA-Z])
            printf "\nletter\n"
            ;;
        [0-9])
            printf "\nDigit\n"
            ;;
        [0-9])
            printf "\nDigit\n"
            ;;
        [,.?!])
            printf "\nPunctuation\n"
            ;;
        *)
            printf "\nerror\n"
    esac
```

# 參考
[Shell case in语句详解](https://c.biancheng.net/view/2767.html)