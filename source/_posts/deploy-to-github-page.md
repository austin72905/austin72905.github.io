---
title: 將Hexo Blog 部屬到github pages 上
date: 2023-7-17 21:25
categories: Git
tags:
    - git
---

## 前言

撰寫完文章後，下一步就是將網站部屬到github pages上了，github 除了可以存放代碼之外，還提供讓你免費部屬靜態網站的功能，
但還是有一些限制，如respository 容量只能到1G，但在初期文章數量不多時，是個好選擇。

官方提供了兩種部屬方式

## 步驟 (使用一鍵部屬)

1. 創建repository，並將hexo 專案推上去，預設`public`目錄不會被推上去

會有兩種域名呈現方式

* <username>.github.io
* <username>.github.io/repositoryName

如果想要讓blog的域名以 `username`.github.io 呈現，那在創建repository時，repository名稱要取為 `username`.github.io ，`username`為github帳號名 如: austin72905.github.io

如果是`username`.github.io/repositoryName呈現方式，在創建repository時，名稱任意。




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

透過 hexo 一鍵部屬套件部屬後，未來更新代碼推上git後，還是需要

```bash
    hexo clean
    hexo g # 生成靜態文件
    hexo d # 部屬
```
將新代碼部屬上去。

如果要使用Github Action實現持續部屬(CD)的功能，需要使用Hexo 官方提供的另一種方式。


## 步驟 (使用Github Action 部屬到Github Pages) (推薦)

1. 創建github repository

同一鍵部屬步驟1，依照需求調整repository名稱，如: `username`.github.io

2. 在hexo 專案中建立`.github/workflows/pages.yml`

3. 查看你的node 版本

```bash
    node --version  # 得到的結果會類似 (v16.5.3)
```
4. 將以下內容貼到`.github/workflows/pages.yml`， (將 `16` 替換為上個步驟中記下的版本)

```yaml
name: Pages

on:
  push:
    branches:
      - main  # default branch，或是master

jobs:
  pages:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 16.x
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```


3. 將hexo 專案推上去，預設`public`目錄不會被推上去

4. 在儲存庫中前往 **Settings** > **Pages** > **Source**，並將 branch 改為 gh-pages

使用此方式，未來只要有新代碼推上去，就會自動觸發Github Action，將Github Pages 上的網站重新編驛更新。


## 參考文章
[github pages文檔](https://docs.github.com/zh/pages/getting-started-with-github-pages/about-github-pages)

[在 GitHub Pages 上部署 Hexo](https://hexo.io/zh-tw/docs/github-pages)