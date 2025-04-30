---
title: git squash (合併commit)
date: 2025-4-30 14:42
categories: git
tags:
    - git
---

# 常見的使用情境
* 你在開發一個功能，過程中做了很多小 commit（像是 "fix typo"、"adjust padding"、"final fix"）。

* 最後想把這些零碎的 commit 整理成一個乾淨的提交紀錄，比較好讓別人 review 或 merge。

這時就會用到 git squash。

# 怎麼操作

通常是配合 `git rebase -i`（interactive rebase）來使用：

```bash
    git rebase -i HEAD~3
```

會挑選最近的 3 筆 commit 進行互動式編輯。

進入編輯器後，會看到類似下列內容：

```
    pick a111111 feat: 建立登入頁
    pick b222222 fix: 修正錯字
    pick c333333 style: 調整 padding
```

將要合併（squash）的 commit，把 pick 改成 squash：

```
    pick a111111 feat: 建立登入頁
    squash b222222 fix: 修正錯字
    squash c333333 style: 調整 padding

```

儲存並離開編輯器（vim 輸入 `:wq`）

接著 Git 會跳出 commit message 編輯畫面，你可以：

保留一條精簡有意義的訊息（建議這樣做）


再次儲存退出，squash 完成




# Git Squash 範例解析

來看一個例子！

假設你執行以下指令：

```bash
    git rebase -i HEAD~5
```

畫面會出現最近的 5 筆 commit，長這樣：

```
    pick   a111111  建立登入頁
    pick   b222222  修正 typo
    pick   c333333  增加登出功能
    pick   d444444  登入頁樣式調整
    pick   e555555  最後細部調整
```

---

如果你改成這樣：

```
    pick   a111111  建立登入頁
    squash b222222  修正 typo
    pick   c333333  增加登出功能
    squash d444444  登入頁樣式調整
    squash e555555  最後細部調整
```

---

## ✅ 這個設定代表什麼？

- `b222222`（修正 typo）會被合併到 `a111111`（建立登入頁）
- `d444444`（樣式調整）和 `e555555`（細部調整）會被合併到 `c333333`（增加登出功能）

---

## 🔔 重點規則（超重要）

| 動作   | 說明                                               |
|--------|----------------------------------------------------|
| pick   | 當作一個新的基礎 commit                             |
| squash | 把這一行的 commit，合併到上面最近的一個 `pick` commit |

所以：

> 不是一律往最上面那個 `pick`，  
> 而是往「**它上面最近的 pick**」合併！

---

✅ 這種技巧非常適合整理功能分支的多個 commit，讓 PR 紀錄更乾淨、更容易閱讀！



# 注意事項
* 建議只在 自己負責的分支 使用（未合併前），避免影響他人。

* `squash` 會將該 commit 合併到 上面最近的 pick commit。

* 最上面那一筆 commit 不能是 `squash`，否則會出錯。

* `squash` 會 改變 commit 歷史與 hash 值，原本的 commit 會被取代。

* 若該分支曾經 push 到遠端，squash 後需要：

```bash
    git push --force

```
