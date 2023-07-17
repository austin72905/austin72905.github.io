---
title: 將Hexo Blog 部屬到github pages 上
date: 2023-7-17 21:25
categories: 技術
tags:
    - git
---

## 前言

撰寫完文章後，下一步就是將網站部屬到github pages上了，github 除了可以存放代碼之外，還提供讓你免費部屬靜態網站的功能，
但還是有一些限制，如respository 容量只能到1G，但在初期文章數量不多時，是個好選擇。

[github pages文檔](https://docs.github.com/zh/pages/getting-started-with-github-pages/about-github-pages)

[在 GitHub Pages 上部署 Hexo](https://hexo.io/zh-tw/docs/github-pages)

官方提供了兩種部屬方式，本文介紹的為一鍵部屬的方式

## 步驟

1. 創建repository，並將hexo 專案推上去，預設`public`目錄不會被推上去

2. 將`_config.yml` 修改:

```yaml
    deploy:
    type: git
    repo: git@github.com:austin72905/austin72905.github.io.git
    branch: gh-pages # 指定github pages 編譯的分支，不要使用master

```

***branch分支不要指定master!!***
如果branch指定master，hexo 專案會把`public`目錄的內容推到master 上，蓋掉你原本推上去的內容，這樣其他的檔案就無法做版本控制了

* master : 未打包的原始碼
* gh-pages : 打包過後的靜態文件

github pages 編譯時針對gh-pages 上的文件進行編譯

3. 下載一鍵部屬套件

```bash
    npm install hexo-deployer-git --save
```

4. 部屬

```bash
    hexo d
```

透過 hexo 一鍵部屬套件部屬後，未來只要更新代碼推上git後，就會自動觸發github Action，將最新的代碼編譯到github pages 的網站上，實現持續部屬(CD)的功能。