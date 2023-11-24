---
title: Linux 系統管理-常用指令(date)
date: 2023-11-5 13:47
categories: Linux
tags:
    - Linux
---


# date (取得系統時間)

可以取得系統時間，在root 權限下，可以修改系統的時間，在撰寫腳本時用來判斷日期挺實用的

### 語法格式

```bash
    date -d "時間字串"  "時間格式化"
```

- 如果沒有指定時間，就會以當前時間為準
- 可以在時間字串指定一個時間，就會以該時間為準

常用參數
* `-d`： 指定一個時間字串

### 時間字串

以下是常用的範例

除了可以用 +、-，也可以用ago、next、yesterday這些單字表示

```bash
    "1 day ago"

    "5 minutes ago"

    "next month"

    "2023/10/15  1 day ago"  # 2023/10/14
```

### 時間格式化

```bash

    "+%Y-%m-%d %H:%M:%S"  # 2009-02-13 23:02:30

    "+%Y/%m/%d %H:%M:%S"  # 2009/02/13 23:02:30

    "+%Y-%m-%d"   #  2009-02-13
```

### 使用範例

```bash

    date "+%Y-%m-%d"  # 今天日期 2023-11-05

    date -d "1 day ago" "+%Y-%m-%d"  # 昨天日期 2023-11-04

    date -d "+1 day" "+%Y-%m-%d"  # 明天日期 2023-11-06

    date -d "next month" "+%Y/%m/%d"  #  下個月 2023-12-05

    date -d "2023/10/03 2 days ago" "+%Y-%m-%d"  #  2023-10-01


```


# 參考資料
[Linux date 命令](https://www.runoob.com/linux/linux-comm-date.html)