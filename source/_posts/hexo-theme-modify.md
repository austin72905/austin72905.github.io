---
title: Hexo 主題修改(Fluid)
date: 2023-7-17 19:17
categories: 技術
tags:
    - 前端
---

# 前言
配置完hexo 主題後，發現很多地方跟自己想要呈現的方式不太一樣，這邊就要來客製化一下了，介紹常見的修改

配置文件都在`themes` 目錄中`fluid/_config.yml`。

## 整個頁面
### 修改左上角，blog 的名稱

[博客标题](https://hexo.fluid-dev.com/docs/guide/#%E5%8D%9A%E5%AE%A2%E6%A0%87%E9%A2%98)

```bash
    navbar:
        blog_title: blog標題
```


### 修改右上角，巡航列的標題

[巡航菜單](https://hexo.fluid-dev.com/docs/guide/#%E5%AF%BC%E8%88%AA%E8%8F%9C%E5%8D%95)

可以看到文檔說明`key` 用于关联有语言配置，如不存在关联则显示 key 本身的值，
在這邊修改key的話，在文章內頁標題的文字不會改變。

如:我想要把"歸檔"兩個字，修改為"全部文章"，當導覽到全部文章頁面時，標題也要顯示全部文章，要達成此效果
要直接修改`languages/zh-TW.yml`
修改完如下

```yaml
    archive:
        menu: '全部文章'
        title: '全部文章'
        subtitle: '全部文章'
        post_total: '共有 %d 篇文章'
```


## 主頁
### 修改主頁的slogan

[修改slogan](https://hexo.fluid-dev.com/docs/guide/#slogan-%E6%89%93%E5%AD%97%E6%9C%BA)

```yaml
    index:
        slogan:
            enable: true
            text: 修改成你要的slogan
```


### 修改頁面頂部大圖

[修改頁面頂部大圖](https://hexo.fluid-dev.com/docs/guide/#%E9%A1%B5%E9%9D%A2%E9%A1%B6%E9%83%A8%E5%A4%A7%E5%9B%BE)

```yaml
    index:
        banner_img_height: 60 # 修改圖片高度
```


## footer
### footer 文字

```yaml
    footer:
        content:
        `
        內容
        `

```


## 文章內頁
### 文章頁顯示訊息(字數、日期..)

[日期/字数/阅读时长/阅读数](https://hexo.fluid-dev.com/docs/guide/#%E6%97%A5%E6%9C%9F-%E5%AD%97%E6%95%B0-%E9%98%85%E8%AF%BB%E6%97%B6%E9%95%BF-%E9%98%85%E8%AF%BB%E6%95%B0)

```yaml
    post:
        meta:
            author:  # 作者，优先根据 front-matter 里 author 字段，其次是 hexo 配置中 author 值
                enable: false
            date:  # 文章日期，优先根据 front-matter 里 date 字段，其次是 md 文件日期
                enable: true
                format: "dddd, MMMM Do YYYY, h:mm a"  # 格式参照 ISO-8601 日期格式化
            wordcount:  # 字数统计
                enable: true
                format: "{} 字"  # 显示的文本，{}是数字的占位符（必须包含)，下同
            min2read:  # 阅读时间
                enable: true
                format: "{} 分钟"
            views:  # 阅读次数
                enable: false
                source: "leancloud"  # 统计数据来源，可选：leancloud | busuanzi   注意不蒜子会间歇抽风
                format: "{} 次"
```

### 代碼塊不顯示使用語言

[代码块](https://hexo.fluid-dev.com/docs/guide/#%E4%BB%A3%E7%A0%81%E5%9D%97)

`fluid/_config.yml`搜尋"代码块的增强配置"

```yaml
    code:
        # 是否开启复制代码的按钮
        # Enable copy code button
        copy_btn: true

    # 代码语言
    # Code language
    language:
        enable: false # 把它關掉
        default: "TEXT"

```


## 關於
### 關於的內容

[关于信息](https://hexo.fluid-dev.com/docs/guide/#%E5%85%B3%E4%BA%8E%E4%BF%A1%E6%81%AF)

```yaml
    about:
    avatar: /img/avatar.png
    name: "你的名字"
    intro: "你的介紹"

```

至於其他的配置，日後如果有修改的需求，會再持續記錄到此文章

## 參考文章

[fluid官方文檔](https://hexo.fluid-dev.com/docs/guide/)