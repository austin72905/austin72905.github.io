---
title: MySQL 「欄位名、表名、資料的值」對大小寫是否敏感
date: 2025-4-15 14:19
categories: MySQL
tags:
    - MySQL
---

## 🔸1. 表名（Table Name）大小寫敏感性

| 作業系統 | 預設行為                             | 說明                       |
|----------|--------------------------------------|----------------------------|
| Linux    | 大小寫敏感 (`lower_case_table_names=0`) | `users` ≠ `Users`          |
| Windows  | 大小寫不敏感 (`lower_case_table_names=1`) | `users` = `Users`          |
| macOS    | 通常不敏感（視檔案系統）                  | 可能為 1 或 2，需確認設定   |

### ✅ 相關參數說明：

- `lower_case_table_names=0`：保留原始大小寫，查詢時大小寫敏感（Linux 常見）
- `lower_case_table_names=1`：強制轉小寫儲存，查詢時大小寫不敏感（Windows）
- `lower_case_table_names=2`：保留原始大小寫儲存，查詢時大小寫不敏感（某些 macOS）


### 📌 查詢 lower_case_table_names 設定值

```sql
SHOW VARIABLES LIKE 'lower_case_table_names';
```

### 🔍 查詢結果說明：

| Variable_name           | Value         |
|-------------------------|---------------|
| `lower_case_table_names` | 0 / 1 / 2     |

### 🔸對應值解釋：

| 值 | 說明                                                   |
|----|--------------------------------------------------------|
| 0  | 表名大小寫敏感（Linux 常見）                            |
| 1  | 表名大小寫不敏感，**所有表名儲存為小寫**（Windows 常見） |
| 2  | 表名大小寫不敏感，但**保留原始大小寫儲存**（macOS 某些情況） |


---

## 🔸2. 欄位名（Column Name）大小寫敏感性

- ✅ 欄位名稱在所有作業系統中預設都是大小寫 **不敏感**
- 範例：`SELECT name` 與 `SELECT NAME` 等價

---

## 🔸3. 別名（Alias）大小寫敏感性

- ✅ SQL 查詢中的別名（例如 `AS Name`）通常不敏感
- ❗ 但應用程式端讀取資料時，**有時會區分大小寫**

---

## 🔸4. 資料值（Data Values）大小寫敏感性

| 資料型別             | 預設大小寫敏感性                  |
|----------------------|-----------------------------------|
| `CHAR`, `VARCHAR`, `TEXT` | ❌ 視 Collation 而定             |
| `BINARY`, `VARBINARY`     | ✅ 大小寫敏感                     |

### ✅ Collation 決定是否敏感：

- `utf8mb4_general_ci` → `ci` = case-insensitive（不敏感）
- `utf8mb4_bin` → `bin` = binary（大小寫敏感）

---

## utf8mb4_general_ci 跟 utf8mb4_0900_ai_ci 差異?

## ✅ Collation 名稱結構說明

| 名稱部分         | 說明                                           |
|------------------|------------------------------------------------|
| `utf8mb4`        | 使用 UTF-8 編碼，可支援完整 Unicode（含 Emoji）|
| `general / 0900` | 指排序與比較邏輯                              |
| `ci`             | Case-insensitive（不區分大小寫）              |
| `ai`             | Accent-insensitive（不區分重音，例如 é = e） |

---

## 🔍 差異總覽

| 特性               | `utf8mb4_general_ci`              | `utf8mb4_0900_ai_ci`                         |
|--------------------|----------------------------------|----------------------------------------------|
| 發佈版本           | MySQL 5.x 以前就有               | ✅ MySQL 8.0 新增                            |
| 語言支援準確度     | ❌ 基本排序，略為不準確           | ✅ 更符合 Unicode 標準                       |
| 重音符區分         | ❌ 不支援 AI，不精準              | ✅ 支援 AI，可分辨 `e` vs `é`（若使用 `_as_ci`）|
| Unicode 規則相容   | ❌ 不完全相容                     | ✅ 遵循 Unicode Collation Algorithm (UCA) 9.0 |
| 效能               | ✅ 較快（因為簡單）               | ❌ 較慢（但差異通常不大）                   |
| 建議用途           | 舊系統 / 無特別語言需求           | 🔥 新系統 / 精準排序需求（推薦）            |

---

## ✅ 建議使用情境

| 情境                 | 推薦 Collation           |
|----------------------|--------------------------|
| 新專案、MySQL 8 以上 | `utf8mb4_0900_ai_ci` ✅   |
| 舊系統升級           | `utf8mb4_general_ci` ✅   |

---

## ✨ 範例比較

| 比較項目             | `utf8mb4_general_ci` | `utf8mb4_0900_ai_ci`              |
|----------------------|----------------------|-----------------------------------|
| `'a' = 'A'`           | ✅ 是                | ✅ 是                             |
| `'e' = 'é'`           | ✅ 是                | ✅ 是（ai）                       |
| `'e' = 'é'` (`as_ci`) | ❌ 否                | ❌ 否（可指定 accent sensitive） |

---

## 📌 查詢資料表或欄位的 Collation

```sql
-- 查詢某資料表欄位的 collation
SHOW FULL COLUMNS FROM your_table;

-- 查整張表的預設 collation
SHOW TABLE STATUS WHERE Name = 'your_table';
```

👉 如果你希望排序更準確，支援更多語言與特殊符號比對（如德文 ß、西班牙 ñ），**建議使用 `utf8mb4_0900_ai_ci`。**

---

## ✅ 總結表

| 項目     | 是否大小寫敏感     | 備註                                  |
|----------|--------------------|---------------------------------------|
| 表名     | 依作業系統與設定   | Linux 敏感、Windows 不敏感            |
| 欄位名   | ❌ 不敏感           | 預設不區分大小寫                      |
| 資料值   | 視 Collation 而定  | `*_ci` 不敏感，`*_bin` 敏感            |