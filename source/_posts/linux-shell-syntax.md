---
title: shell 語法介紹
date: 2023-11-27 09:28
categories: Linux
tags:
    - Linux
    - shell
---


# shell 語法

可以分成4個部分

- 使用的shell：在腳本文件的第一行可以指定shell 的指示器，通常都是用`#!/bin/bash`
- 註解：使用`#` 可以註解
- 命令：要腳本做的動作，echo、cp…
- 判斷式：if、for、while…

創建shell 腳本時，記得給予執行權限(x)


# 變數

## 定義變數、呼叫變數
定義變數後(定義時不需要$)，使用$呼叫變數
``` bash
    #!/bin/bash

    # 這是註解 ，第一行!之後不要有空格

    a="cool popo"
    echo "test $a"
```
**Output**：
``` 
    test cool popo
```

## 將指令運行結果保存到變數
定義變數時，如果想要將linux 指令運行的結果存到變數，要加上``
或是使用`$()`
``` bash
    #!/bin/bash

    # 這是註解 ，第一行!之後不要有空格

    a=`hostname`
    echo "Hi my server name is $a"

    s=$(ls -l)

    echo "result=$s"
```

# 數值計算 (())

* 如果想要數值計算，可以用`(())` 
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

* if 要進行數值比較時

``` bash
    #!/bin/bash
    a=5
    b=10

    if ((a < b)); then
        echo "$a 小于 $b"
    else
        echo "$a 不小于 $b"
    fi
```

# 創建子shell ()

創建子shell原因
* 改變子shell 中的變數，不會影響到父shell，當想要限制變數的作用域時可以使用
* 可以執行一系列指令

``` bash
    #!/bin/bash
    variable=42

    (
        echo "在子shell中，variable 的值是 $variable" 
        variable=99
        echo "在子shell中，修改后的 variable 的值是 $variable"
    )

    echo "在父shell中，variable 的值仍然是 $variable"
```


``` bash
    #!/bin/bash
    # 在子shell中执行一系列命令
    (
        echo "这是子shell中的命令1"
        echo "这是子shell中的命令2"
        # 更多命令...
    )

    # 在父shell中执行的命令
    echo "这是父shell中的命令"
    
```

# 保留制表符

使用 `""` 包住變數

有一文件`o_content`

```
    ax-aa
    bx-bb
    cx-cc
    dx-dd
```


腳本
```bash
    #!/bin/bash


    # 使用"" 包住變數可以保留制表符 \n \t 那些

    # 如果沒有用 "" 包住 

    result=`cat o_content | awk -F '-' '{print $2}'`

    echo $result

```

Output  (會以空格分隔，不是如預期一般以`\n`分隔)
```
    aa bb cc dd
```

如欲想要以`\n`分隔，使用 `""` 包住變數
```bash
    #!/bin/bash
    result=`cat o_content | awk -F '-' '{print $2}'`

    echo "$result"
```
Output
```
    aa
    bb
    cc
    dd
```