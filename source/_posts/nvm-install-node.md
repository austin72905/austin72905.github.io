---
title: 使用nvm 安裝 node.js
date: 2023-12-16 00:06
categories: Node.js
tags:
    - Node.js
---

# 前言

nvm (Node Version Manager)為 node.js 的版本管理工具，透過nvm，能夠在同一台電腦上輕鬆的管理多個版本的node.js，可以依照每個專案的需求切換node.js的版本


# nvm安裝步驟

Windows 版本有提供下載工具[nvm-windows](https://github.com/coreybutler/nvm-windows)
[下載連結](https://github.com/coreybutler/nvm-windows/releases)滑到下面選擇`nvm-setup.zip`
，點選後就一路按下一步就能順利安裝

接著在終端輸入`nvm`，確認是否有安裝成功

# node 安裝

安裝完`nvm` 後接著就可以下載 node.js，下載node.js預設就會跟著下載套件管理工具`npm`。

輸入<font color=#EB5757>`nvm install node的版本號`</font>下載指定版本的node.js，版本號可以去node.js官網查看LTS，如:<font color=#EB5757>`nvm use 20.10.0`</font>
下載完可以用`node -v` 檢查是否有安裝成功

# 'node' 不是內部或外部命令、可執行的程式或批次檔。
下載完之後如果出現以上訊息
使用 `nvm use 安裝node的版本號`

如:`nvm use 20.10.0`

不知道為啥使用了他沒有預設use我安裝的版本，還要自己指定...

再次使用`node -v` 發現可以正常使用了

# 參考

[安裝 nvm 環境，Node.js 開發者必學（Windows、Mac 均適用）](https://www.casper.tw/development/2022/01/10/install-nvm/)