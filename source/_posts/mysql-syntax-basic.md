---
title: MySQL 常用語法整理
date: 2025-4-10 03:56
categories: MySQL
tags:
    - MySQL
---

# MySQL 常用語法整理

以下範例以 `users` 資料表為例，欄位包含：`id`, `name`, `email`, `age`, `created_at`。

---

## ✅ 插入資料（INSERT）

```sql
INSERT INTO users (name, email, age)
VALUES ('小明', 'xiaoming@example.com', 25);
```

---

## ✅ 查詢資料（SELECT）

```sql
SELECT * FROM users;
```

### 查詢特定欄位

```sql
SELECT name, email FROM users;
```

---

## ✅ 更新資料（UPDATE）

```sql
UPDATE users
SET age = 26
WHERE name = '小明';
```

---

## ✅ 刪除資料（DELETE）

```sql
DELETE FROM users
WHERE name = '小明';
```

---

## ✅ 排序資料（ORDER BY）

```sql
SELECT * FROM users
ORDER BY age DESC;
```

---

## ✅ 限制回傳筆數（LIMIT）

```sql
SELECT * FROM users
LIMIT 1;
```

---

## ✅ 結合條件 + 排序 + LIMIT 1

### 取得年紀最大的使用者

```sql
SELECT * FROM users
ORDER BY age DESC
LIMIT 1;
```

### 取得最新註冊的使用者（假設有 created_at 欄位）

```sql
SELECT * FROM users
ORDER BY created_at DESC
LIMIT 1;
```

---

# MySQL 批量插入與批量更新語法

---

## ✅ 批量插入（Bulk Insert）

```sql
INSERT INTO users (name, email, age)
VALUES 
  ('小明', 'xiaoming@example.com', 25),
  ('小華', 'xiaohua@example.com', 30),
  ('小美', 'xiaomei@example.com', 22);
```

---

## ✅ 批量更新（Bulk Update）方式一：使用 CASE WHEN

> 假設我們要依據 id 更新 age 欄位：

```sql
UPDATE users
SET age = CASE id
  WHEN 1 THEN 26            -- 就是 set age = 26 where id = 1
  WHEN 2 THEN 31
  WHEN 3 THEN 23
END
WHERE id IN (1, 2, 3);
```

---

## ✅ 批量更新（Bulk Update）方式二：JOIN 另一張臨時表

> 可用於複雜更新情境（例如來自另一張表的資料）

```sql
UPDATE users u
JOIN (
  SELECT 1 AS id, 26 AS age
  UNION ALL
  SELECT 2, 31
  UNION ALL
  SELECT 3, 23
) tmp ON u.id = tmp.id
SET u.age = tmp.age;
```

---

✅ 建議使用方式：

- **少量更新** → 使用 `CASE WHEN`
- **大量更新且來自其他資料來源** → 使用 `JOIN`

---

# MySQL 常用聚合函數與日期函數

---

## ✅ 聚合函數（Aggregate Functions）

```sql
-- 總筆數
SELECT COUNT(*) FROM users;

-- 某欄位不為 NULL 的筆數
SELECT COUNT(email) FROM users;

-- 總和
SELECT SUM(age) FROM users;

-- 平均值
SELECT AVG(age) FROM users;

-- 最大值
SELECT MAX(age) FROM users;

-- 最小值
SELECT MIN(age) FROM users;

-- 分組統計（以性別分組計算平均年齡）
SELECT gender, AVG(age)
FROM users
GROUP BY gender;
```

---

## ✅ 日期函數（Date Functions）

```sql
-- 取得今天日期
SELECT CURDATE();             -- 2025-04-10

-- 取得現在日期與時間
SELECT NOW();                 -- 2025-04-10 14:30:00

-- 只取時間
SELECT CURTIME();             -- 14:30:00

-- 字串轉日期
SELECT STR_TO_DATE('2025-04-10', '%Y-%m-%d');

-- 日期加減
SELECT DATE_ADD(NOW(), INTERVAL 7 DAY);      -- 加7天
SELECT DATE_SUB(NOW(), INTERVAL 1 MONTH);    -- 減1個月

-- 擷取部分時間資訊
SELECT YEAR(created_at) AS year,
       MONTH(created_at) AS month,
       DAY(created_at) AS day,
       HOUR(created_at) AS hour,
       MINUTE(created_at) AS minute
FROM users;

-- 格式化時間
SELECT DATE_FORMAT(created_at, '%Y/%m/%d %H:%i:%s') AS formatted_time
FROM users;
```

---

## ✅ 結合範例：統計每月註冊人數

```sql
SELECT DATE_FORMAT(created_at, '%Y-%m') AS register_month,
       COUNT(*) AS user_count
FROM users
GROUP BY register_month
ORDER BY register_month DESC;
```

---