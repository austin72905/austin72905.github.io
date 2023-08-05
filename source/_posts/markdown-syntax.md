---
title: Markdown 常見語法整理
date: 2023-8-05 14:24
categories: Markdown
tags:
    - Markdown
    - 網頁
---

# 前言

Markdown 是一種標記語言，允許使用者使用簡單的標記，就能寫出排版工整，畫面整齊俐落的文章。不用記住繁雜的語法，讓使用者可以更專注在寫作內容，而且源文件人類容易閱讀，不會被複雜格式擾亂。

Markdown檔案可以輕鬆轉變為HTML 文件在網頁上呈現，很適合用來寫部落格。這邊整理幾個常用的標記語法。


# 標題

使用 `# 標題` (# 與 文字間需要空一個)，會對應到HTML 中 H1~H6 的標題大小

``` bash
# 標題     (H1)
##  標題
###  標題
####  標題
#####  標題
###### 標題 (H6)

```

# 反白字體

使用`` 包住文字(調成英文輸入比較方便)，就能有反白效果


`反白文字`

# 項目

使用 `- 項目` 或是 `* 項目` 或是 ‵`數字. 項目` (與文字間需要空一格)，還可以縮排 (使用文字編輯器時按 tab 縮排就行了)

* 項目1
* 項目2
* 項目3

- A
- B
- C

1. 甲
2. 乙
3. 丙
    * 你
    * 好
    * 棒

# 程式碼

技術文章常需要展示代碼範例，可以用 ` ``` ` 換行後包住，來表示代碼塊，支持多種程式語言

` ```程式語言 ` 
` ``` `  

    
例如使用bash的語法高亮，代碼塊結果如下:
```bash
    #  ```bash
    #
    #     ...程式碼內容
    #
    #
    #  ```

```

# 超連結

使用 `[ ]` 包住顯示文字、與 `( )` 包住網址連結 

```
[顯示文字](網址連結)

```

超連結 結果如下:

[顯示文字](https://markdown.tw/)



# 圖片

與超連結語法很像，前面多了一個 `!`

```
![圖片失效時代替文字](圖片連結)

```
圖片 結果如下:
![圖片失效時代替文字](圖片連結)


## 配合google drive 裡面的圖片

google drive 裡面圖片的連結

1. google drive 裡面圖片的連結通常會是  **https://drive.google.com/file/d/XXXXXX/view?usp=sharing** 這種格式

2. 把網址換成 **https://drive.google.com/u/2/uc?id=XXXXX&export=download**

```
    ![圖片介紹](https://drive.google.com/u/2/uc?id=XXXXX&export=download)
```



# 表格


```
    | 列 1 |列 2| 列 3 | 列 4 | 列 5 | 列 6 | 列 7 | 列 8 |
    | ---- |   ----   |  ----: | ----  | ---- | ----  | ----| :----: |
    | drwxr-xr-x. | 21|rootgg|rootgg|4096|Feb|27|13:33|
    | lrwxrwxrwx. | 1|root|root|7|Feb|27|13:15|
    | -rw-r-r-- | 1|root|root|0|Mar|27|13:15|
```

結果如下

| 列 1 |列 2| 列 3 | 列 4 | 列 5 | 列 6 | 列 7 | 列 8 |
| ---- |   ----   |  ----: | ----  | ---- | ----  | ----| :----: |
| drwxr-xr-x. | 21|rootgg|rootgg|4096|Feb|27|13:33|
| lrwxrwxrwx. | 1|root|root|7|Feb|27|13:15666|
| -rw-r-r-- | 1|root|root|0|Mar|27|13:15|


排版預設向左對齊，可以調整第二行的`----`來變更
* `:----`   左對齊
* `----:`    右對齊
* `:----:`    置中對齊

# 刪節線

使用`---` 來表示刪節線

刪節線結果如下:

---


# 粗體、斜體

*斜體*
**粗體**
***粗斜體***
~~刪除線~~
<u>底線</u>

```md
    *斜體*
    **粗體**
    ***粗斜體***
    ~~刪除線~~
    <u>底線</u>
```

# 文字顏色

使用 `<font>`標籤包住文字，`<font color=#EB5757>字體顏色</font>`

<font color=#EB5757>字體顏色</font>


可以搭配反白文字、粗體等等使用 
<font color=#EB5757>`字體顏色`</font>

<font color=#EB5757>**`字體顏色`**</font>

```md

    搭配反白
    <font color=#EB5757>`字體顏色`</font>

    搭配粗體
    <font color=#EB5757>**`字體顏色`**</font>
```

# 引言

使用 `> 文字` (與文字間需要空一格)

結果如下

> 我把妹用這招從來沒有失敗過。    - -*美國隊長*

# 參考資料

[Markdown 常用語法整理](https://sam.webspace.tw/2020/01/10/Markdown%20%E5%B8%B8%E7%94%A8%E8%AA%9E%E6%B3%95%E6%95%B4%E7%90%86/)

[[教學] 撰寫 Hexo 文章 - Markdown 語法大全](https://ed521.github.io/2019/08/hexo-markdown/)

[[實用]建立Google Drive圖床並顯示到網站上的方法](https://www.mytechgirl.com/tw/cloud/how-to-create-google-drive-online-photo-album-mtg6688.html)

[Markdown 放入 google drive 圖片](https://theriseofdavid.github.io/2021/02/28/blog/hackmd_view_googledricePIC/)