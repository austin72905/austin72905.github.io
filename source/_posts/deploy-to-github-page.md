---
title: 將hexo blog 部屬到github pages 上
date: 2023-7-17 21:25
categories: 技術
tags:
    - git
---

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
    branch: gh-pages # 如果是使用 http(s)://<username>.github.io 這邊就填master

```

3. 下載一鍵部屬套件

```bash
    npm install hexo-deployer-git --save
```

4. 部屬

```bash
    hexo d
```
