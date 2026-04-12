---
title: Git 常用指令與分支流程
date: 2026-04-12 16:25
categories: git
tags:
  - git
---

# 前言
這篇整理我在團隊開發最常用的 Git 指令，以及 `main`、`release`、`dev` 三條主要分支的協作方式。

# 常用指令速查

## `git rebase`
把目前分支的提交，改接到另一個基底之後，讓提交歷史更線性。(建議拉取同分支的改動再用)

```bash
git checkout feature/login
git rebase dev
```

適合情境：
- 功能分支想跟上 `dev` 最新進度。
- 想減少無意義的 merge commit。
- 通常是你在同個分支有commit，然後同分支pull 下來不想有merge 紀錄，可以用rebase ; 跨分支再用merge

注意：
- 已經推到遠端且多人共用的分支，不建議隨意 rebase（會改寫歷史）。

## `git merge`
把來源分支的歷史合併到目前分支，保留分叉紀錄。

```bash
git checkout dev
git merge feature/login
```

適合情境：
- 團隊協作時保留完整分支脈絡。
- 要把功能分支正式整合進主幹分支。

## `git reset`
移動 `HEAD`，可用於回退提交或取消暫存。

```bash

# 回退一個commit（保留工作區變更）
git reset --soft HEAD~1 

--hard 則是直接取消指個commit 不會回到工作區
```

適合情境：
- commit 後發現要補內容，先退回再重整。 


## `git revert`
建立一個「反向操作」來撤銷某次commit，不改寫歷史。例如某個commit新增了app.js， 那revert 就會反向操作 - app.js

```bash
git revert <commit-hash>
```

適合情境：
- 已推到共享分支，且要安全撤銷變更。
- 正式環境出包，需要可追蹤地回滾。

## `git pull`
抓取遠端更新並整合到目前分支。

```bash
git pull origin dev
```

適合情境：
- 開工前先同步最新進度。

## `git push`
把本地提交推到遠端分支。

```bash
git push origin feature/login
```

適合情境：
- 功能完成要發 PR / MR。
- 與團隊共享最新提交。

## `git cherry-pick`
把指定 commit 複製到目前分支。

```bash
git checkout release
git cherry-pick <commit-hash>
```

適合情境：
- Hotfix 只想挑一筆修正進 `release` 或 `main`。
- 不想整包 merge 只取特定提交。

## `git stash`
暫存目前未提交變更，讓工作區先回到乾淨狀態。

```bash
git stash
git checkout another-branch
# 回來後
git stash pop
```

適合情境：
- 正改到一半，臨時需要切分支救火。
- 不想為了切分支先做不完整 commit。

## `git commit --amend`
修改最近一次 commit（訊息或內容）。

```bash
git add .
git commit --amend
```

適合情境：
- 剛提交就發現漏檔、commit message 打錯。
- sourcetree 可以commit 前勾選右下角的amend 就會跟上個commit 合併

注意：
- 若該 commit 已推到遠端，amend 後推送通常需 `--force-with-lease`，先確認團隊規範。

# Merge Request（或 Pull Request）流程
1. 從 `dev`（或規範的基底分支）切出功能分支。
2. 開發完成後推到遠端。
3. 建立 MR/PR，請同事 review。
4. 通過 review 與 CI 後再 merge。
5. merge 完刪除功能分支，保持分支整潔。

# 分支策略（`main` / `release` / `dev`）

## 角色定義
- `main`：正式上線版本，永遠保持可部署。
- `release`：測試中的候選版本（例如準備 1.3）。
- `dev`：日常開發整合分支（例如開發 1.4）。

## 版本流向
- 正常開發路徑：`dev` -> `release` -> `main`
- 回補同步路徑：`main` -> `release` -> `dev`

## 典型流程
1. 新功能先合進 `dev`。
2. 版本封版時，將 `dev` 合到 `release` 交給 QA 驗證。
3. 測試通過後，`release` 合到 `main` 上線。
4. 上線後把 `main` 的變更回補到 `release`、`dev`，避免分支漂移。

## Hotfix 流程
1. 線上緊急問題從 `main` 拉出 `hotfix/*` 分支。
2. 修完後先合回 `main` 並上線。
3. 以 `merge` 或 `cherry-pick` 把同一修正帶回 `release`、`dev`。

# 什麼時候用 `merge`、`rebase`、`cherry-pick`
- 優先 `merge`：團隊協作主流程，保留歷史最安全。
- 適度 `rebase`：只在個人功能分支整理歷史用。或是同分支pull 下來時減少merge 紀錄
- 精準 `cherry-pick`：跨分支帶單一修正（尤其 hotfix）。


