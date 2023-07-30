---
title: Hexo 框架介紹
date: 2023-7-17 16:30
categories: Hexo
tags:
    - 前端
---



# 前言

Hexo 是一個blog 框架，用於快速產生"靜態"網站，基於 node.js，使用markdown 撰寫文章
以下為官網連結 [Hexo](https://hexo.io/zh-tw/)

# 安裝流程

1. 首先要先安裝node.js

2. 安裝hexo cli，才能使用hexo 指令

```bash
    npm install hexo-cli -g
```

3. 建立blog 專案
    
```bash
    # 沒有加<folder>會在當前資料夾建立hexo 專案
    # 也可以自行指定資料夾
    hexo init <folder>
```

4. 運行 blog 專案

```bash
    hexo server
```

看到專案建立起來後，接下來就可以來撰寫文章了，以下就來介紹幾個重要的文件

# 目錄結構

* scaffold : 文章的範例模板
* source/_post : 文章內容所在
* themes : 可以將網路上下載的主題放入此資料夾

# 文章範例

```bash
    ---
    title: 標題
    date:  日期 # 依照文章當下新增的時間，但如果之後不小心資料損毀，就無法知道確切時間，所以建議自己打日期
    tags:      ## 如果只有單一標籤，直接寫在貿號後面就好
        -  前端
    ---

    # 以下開始為內文

    # 這會是一個超連結
    [文字](http連結)


    `此為行內程式碼`

    ```使用的語言，如html
    此為段落型程式碼
    ``` # 使用兩塊 ``` 包起來
    


```

# 部屬到線上

1. 編譯 blog 專案，編譯完會出現一個public 目錄

```bash
    hexo g
```

2. 只要將生成的public 資料夾，移動到任意web server 裡面，就能夠運行起來


# 部屬到github


# 調整themes

到網路上搜尋喜歡的主題，按照指示操作，通常都會是以下步驟

1. 到github 下載該主題的原始碼，下載成zip檔較好操作

2. 將壓縮檔解壓縮，裡面的檔案放入blog 專案中 themes 目錄，並將名稱改為`${主題指定的名稱}`

3. 有時會要求額外下載套件

4. 到blog 專案根目錄的<font color=#FF2D2D>`_config.yml`</font>，將`theme`屬性修改為`${主題指定的名稱}`

## 實際調整範例

後來看了 [【Day 7】如何更換 Hexo 主題](https://ithelp.ithome.com.tw/articles/10242172) 的文章，決定使用
[fluid-dev/hexo-theme-fluid](https://github.com/fluid-dev/hexo-theme-fluid) 的主題，安裝文檔及修改配置的文檔都寫得很清楚。

以下是使用文檔提供的下載方式二，
下載 `最新 release 版本` 解壓到 `themes` 目錄，並將解壓出的文件夾重命名為 `fluid`。
1. 依照上面步驟建立好blog 專案

2. 下載 `最新 release 版本` 解壓到 `themes` 目錄，並將解壓出的文件夾重命名為 `fluid`。

3. 修改blog 專案根目錄下`_config.yml` :

```bash
    theme : fluid # 指定主題

    language: zh-TW # 指定顯示語言
```

4. 原先hexo官方是沒有關於頁的，本主題有所以需要自己添加

```bash
    hexo new page about
```

創建成功後，在blog 專案底下`/source/about/index.md`，添加`layout` 屬性。

修改後文件內容
```bash
    ---
    title: about
    layout: about
    ---

    #底下也可以寫一些內容
```

5. 重新啟動後主題就會更換

如果有一些配置需要更改可以[查看此篇記錄](https://hexo.fluid-dev.com/docs/guide/)



調整blog 主題配置可能一開始比較費勁，但相比於自己從零開始搞已經節省很多的時間了，也更可以專注在寫作上，感謝開發者的貢獻。
看到自己的blog 有美美的主題就更有動力寫作了! 希望大家寫作愉快。



# 參考文章

[使用 Hexo 建構個人化部落格](https://www.youtube.com/watch?v=jOJI9ekTzK8)

[如何更換 Hexo 主題](https://ithelp.ithome.com.tw/articles/10242172)