---
title: MySQL Join
date: 2025-4-11 13:37
categories: MySQL
tags:
    - MySQL
---


# 多表查詢(JOIN)



# 語法



```sql
SELECT 左表.欄位名, 右表.欄位名
FROM 左表
JOIN 右表 ON 左表.欄位名 = 右表.欄位名;
```



- JOIN 預設為 `INNER JOIN`

- 習慣是使用別名 AS 別名

- 左表 就是 FROM 的表

- 右表 就是 JOIN 的表



```sql

SELECT o.OrderID, c.CustomerName 
  FROM Orders AS o 
JOIN Customers AS c ON (o.CustomerID = c.CustomerID);
```



# JOIN 差異?



+ ## sql join 差異?

   

   SQL 中，`JOIN` 用於將來自兩個或多個表的數據組合在一起。不同類型的 `JOIN` 控制了數據的匹配方式及結果集的內容。以下是常見的 SQL `JOIN` 類型及其差異：

   ---

   ### **1\. INNER JOIN**

   - **功能**：

      - 只返回兩個表中**匹配**的記錄。

   - **特點**：

      - 沒有匹配的記錄將不出現在結果集中。

   **範例：**

   ```sql
   SELECT users.id, users.name, orders.amount 
     FROM users 
    INNER JOIN orders ON (users.id = orders.user_id);
   ```

   

   **解釋**：

   - 只有當 [`users.id`](users.id)` = orders.user_id` 時，記錄才會出現在結果中。

   **圖示**：

   - 返回兩個表的交集部分。

   ---

   ### **2\. LEFT JOIN (LEFT OUTER JOIN)**

   - **功能**：

      - 返回左表中的所有記錄，以及右表中匹配的記錄。如果右表中沒有匹配，則以 `NULL` 填充。

   - **特點**：

      - 即使右表中沒有匹配，左表的數據仍會顯示。

   **範例：**

   ```sql
   SELECT users.id, users.name, orders.amount 
     FROM users 
    LEFT JOIN orders ON (users.id = orders.user_id);
   ```

   

   

   **解釋**：

   - 即使 `orders` 表中沒有與 [`users.id`](users.id) 匹配的 `user_id`，`users` 表的記錄也會出現在結果中。

   **圖示**：

   - 包括左表的所有數據，右表只有匹配的部分。

   ---

   ### **3\. RIGHT JOIN (RIGHT OUTER JOIN)**

   - **功能**：

      - 返回右表中的所有記錄，以及左表中匹配的記錄。如果左表中沒有匹配，則以 `NULL` 填充。

   - **特點**：

      - 即使左表中沒有匹配，右表的數據仍會顯示。

   **範例：**

   ```sql
   SELECT users.id, users.name, orders.amount 
     FROM users 
    RIGHT JOIN orders ON (users.id = orders.user_id);
   ```

   

   

   **解釋**：

   - 即使 `users` 表中沒有與 `orders.user_id` 匹配的 `id`，`orders` 表的記錄也會出現在結果中。

   **圖示**：

   - 包括右表的所有數據，左表只有匹配的部分。

   ---

   ### **4\. FULL JOIN (FULL OUTER JOIN)** ( MSSQL 才有，MySQL要用union模擬 )

   - **功能**：

      - 返回兩個表中的**所有記錄**，++無論是否匹配++。

      - 如果沒有匹配，則以 `NULL` 填充未匹配的部分。

   - **特點**：

      - 左表和右表的數據都會出現在結果中。

   **範例：**

   ```sql
   SELECT users.id, users.name, orders.amount 
     FROM users 
    FULL JOIN orders ON (users.id = orders.user_id);
   ```

   **解釋**：

   - 無論 `users` 表或 `orders` 表中是否有匹配，所有記錄都會出現在結果中。

   **圖示**：

   - 左表和右表的所有數據（聯集），匹配部分為交集。

   
   ### ⚠️ MySQL 不支援 `FULL OUTER JOIN`，需改用 `UNION`

    ```sql
    SELECT * FROM users u
    LEFT JOIN orders o ON u.id = o.user_id

    UNION

    SELECT * FROM users u
    RIGHT JOIN orders o ON u.id = o.user_id;
    ```
   
   ---




   ### **5\. CROSS JOIN (滿少用的感覺)**

   - **功能**：

      - 返回兩個表的**笛卡爾積**，即每個左表記錄與每個右表記錄的組合。

   - **特點**：

      - 結果集的行數等於兩個表的記錄數相乘。

   **範例：**

   ```sql
   SELECT users.id, users.name, orders.amount FROM users CROSS JOIN orders;
   ```

   

   

   **解釋**：

   - 不使用 `ON` 條件，會生成每個 `users` 記錄與每個 `orders` 記錄的組合。

   **圖示**：

   - 兩個表的所有可能組合。

   ---

   ### **6\. SELF JOIN (滿少用的感覺)**

   - **功能**：

      - 將一個表與自身進行 JOIN。
      - 本身沒有 SELF JOIN 這個語法，是說將兩個一樣的表JOIN 的行為

   - **特點**：

      - 常用於有層級結構（如樹形數據）或需要比較同一表內記錄的場景。

   **範例：**

   ```sql
   SELECT e1.id AS EmployeeID, e1.name AS EmployeeName, e2.name AS ManagerName 
     FROM employees e1
    LEFT JOIN employees e2 ON e1.manager_id = e2.id;
   ```

   

   **解釋**：

   - 將 `employees` 表中的員工與其上級經理進行關聯。

   **圖示**：

   - 一個表映射為兩個角色（員工與經理）。

   ---

   ### **比較總結**

   

   | JOIN 類型 | 結果描述 | 常見用途 | 
   |---|---|---|
   | **INNER JOIN** | 僅返回兩個表中匹配的數據 | 獲取多表相關聯數據，確保數據一致。 | 
   | **LEFT JOIN** | 包括左表的所有數據，右表中匹配的數據 | 查找左表中有而右表中可能缺失的數據（**如用戶沒有訂單的情況**）。 | 
   | **RIGHT JOIN** | 包括右表的所有數據，左表中匹配的數據 | 查找右表中有而左表中可能缺失的數據（**如訂單沒有用戶的情況**）。 | 
   | **FULL JOIN** | 包括左右兩個表中的所有數據，未匹配部分填充 `NULL` | 整合所有數據來源，無論是否有匹配。 | 
   | **CROSS JOIN** | 生成兩個表的笛卡爾積 | 不常用，通常用於組合數據或生成測試數據。 | 
   | **SELF JOIN** | 同一表內進行 JOIN，根據條件建立內部關聯 | 適用於層級結構（如員工與經理）、數據比較（**如找出同一表內的重複數據**）。 | 
   

   ---

   ### **實踐建議**

   1. **優先選擇 INNER JOIN**：

      - 當你確定只需要匹配數據時，優先使用 `INNER JOIN`。

   2. **小心使用 CROSS JOIN**：

      - `CROSS JOIN` 可能生成大量數據，注意控制數據量或加條件過濾。

   3. **選擇正確的 JOIN 類型**：

      - 根據需求選擇適當的 JOIN 類型，避免返回多餘或不必要的數據。

   4. **優化 JOIN 條件**：

      - 確保 JOIN 條件（`ON` 子句）正確且有索引支援，避免全表掃描。

   如果有具體的 SQL 查詢需求，歡迎進一步討論！



+ ## 各種join 的返回結果

   以下是各種 **JOIN** 類型對應的返回結果，基於上述的 `Orders` 和 `Customers` 表的數據。

   ---

   ### **表格資料回顧**

   #### 表 `Orders`：

   

   | OrderID | CustomerID | TotalAmount | 
   |---|---|---|
   | 1 | 101 | 500 | 
   | 2 | 102 | 300 | 
   | 3 | 103 | 700 | 
   

   #### 表 `Customers`：

   

   | CustomerID | CustomerName | 
   |---|---|
   | 101 | Alice | 
   | 102 | Bob | 
   

   ---

   ### **1\. INNER JOIN（預設的 JOIN）**

   #### 查詢語句：

   ```sql
   SELECT Orders.OrderID, Orders.TotalAmount, Customers.CustomerName 
     FROM Orders 
    INNER JOIN Customers ON (Orders.CustomerID = Customers.CustomerID);
   ```

   複製程式碼

   

   #### 結果：

   

   | OrderID | TotalAmount | CustomerName | 
   |---|---|---|
   | 1 | 500 | Alice | 
   | 2 | 300 | Bob | 
   

   **解釋：**

   - 僅返回 **鍵值匹配** 的行。

   - `OrderID = 3` 被排除，因為它的 `CustomerID = 103` 在 `Customers` 表中找不到匹配。

   ---

   ### **2\. LEFT JOIN（或 LEFT OUTER JOIN）**

   #### 查詢語句：

   ```sql
   SELECT Orders.OrderID, Orders.TotalAmount, Customers.CustomerName 
     FROM Orders 
    LEFT JOIN Customers ON (Orders.CustomerID = Customers.CustomerID);
   ```

   

   #### 結果：

   

   | OrderID | TotalAmount | CustomerName | 
   |---|---|---|
   | 1 | 500 | Alice | 
   | 2 | 300 | Bob | 
   | 3 | 700 | NULL | 
   

   **解釋：**

   - 返回 `Orders` 表中的所有行。

   - 如果 `Orders.CustomerID` 沒有在 `Customers` 表中找到匹配，`CustomerName` 顯示為 `NULL`。

   ---

   ### **3\. RIGHT JOIN（或 RIGHT OUTER JOIN）**

   #### 查詢語句：

   ```sql
   SELECT Orders.OrderID, Orders.TotalAmount, Customers.CustomerName 
     FROM Orders 
    RIGHT JOIN Customers ON (Orders.CustomerID = Customers.CustomerID);
   ```

   

   

   #### 結果：

   

   | OrderID | TotalAmount | CustomerName | 
   |---|---|---|
   | 1 | 500 | Alice | 
   | 2 | 300 | Bob | 
   

   **解釋：**

   - 返回 `Customers` 表中的所有行。

   - 因為 `Orders` 表中的數據都能匹配到 `Customers`，結果和 `INNER JOIN` 一樣。

   ---

   ### **4\. FULL JOIN（或 FULL OUTER JOIN）**

   #### 查詢語句：

   ```sql
   SELECT Orders.OrderID, Orders.TotalAmount, Customers.CustomerName 
     FROM Orders 
    FULL JOIN Customers ON (Orders.CustomerID = Customers.CustomerID);
   ```

   

   

   #### 結果：

   

   | OrderID | TotalAmount | CustomerName | 
   |---|---|---|
   | 1 | 500 | Alice | 
   | 2 | 300 | Bob | 
   | 3 | 700 | NULL | 
   

   **解釋：**

   - 返回左右兩張表的所有行。

   - 未匹配的行以 `NULL` 填充對應的列。

   ---

   ### **5\. CROSS JOIN**

   #### 查詢語句：

   ```sql
   SELECT Orders.OrderID, Orders.TotalAmount, Customers.CustomerName 
     FROM Orders 
    CROSS JOIN Customers;
   ```

   

   #### 結果：

   

   | OrderID | TotalAmount | CustomerName | 
   |---|---|---|
   | 1 | 500 | Alice | 
   | 1 | 500 | Bob | 
   | 2 | 300 | Alice | 
   | 2 | 300 | Bob | 
   | 3 | 700 | Alice | 
   | 3 | 700 | Bob | 
   

   **解釋：**

   - **笛卡爾積**：將 `Orders` 和 `Customers` 表的每一行組合，結果的行數為 `Orders` 表行數 \* `Customers` 表行數（3 x 2 = 6 行）。

   - 不需要匹配條件，所有可能的組合都會出現。

   ---

   ### **總結對比**

   

   | JOIN 類型 | 結果內容 | 
   |---|---|
   | **INNER JOIN** | 只返回鍵值匹配的行。 | 
   | **LEFT JOIN** | 返回左表的所有行，右表沒有匹配的用 `NULL` 填充。 | 
   | **RIGHT JOIN** | 返回右表的所有行，左表沒有匹配的用 `NULL` 填充。 | 
   | **FULL JOIN** | 返回左右表的所有行，沒有匹配的行用 `NULL` 填充。 | 
   | **CROSS JOIN** | 笛卡爾積，返回左右表的所有行的所有組合。 | 
   

   

   用稍微複雜點的例子

   ###### 

   ### **複雜的 JOIN 範例**

   假設我們有以下三個表：

   ---

   #### 表 `Orders` (訂單表)：

   

   | OrderID | CustomerID | TotalAmount | 
   |---|---|---|
   | 1 | 101 | 500 | 
   | 2 | 102 | 300 | 
   | 3 | 103 | 700 | 
   | 4 | 104 | 200 | 
   

   ---

   #### 表 `Customers` (顧客表)：

   

   | CustomerID | CustomerName | Region | 
   |---|---|---|
   | 101 | Alice | North | 
   | 102 | Bob | South | 
   | 105 | Charlie | West | 
   

   ---

   #### 表 `Products` (產品表)：

   

   | ProductID | OrderID | ProductName | Quantity | 
   |---|---|---|---|
   | 201 | 1 | Laptop | 1 | 
   | 202 | 1 | Mouse | 2 | 
   | 203 | 2 | Keyboard | 1 | 
   | 204 | 3 | Monitor | 1 | 
   | 205 | 5 | Phone | 1 | 
   

   ---

   ### **查詢各種 JOIN 的結果**

   ---

   ### **1\. INNER JOIN（兩表匹配的情況）**

   #### 查詢語句：

   ```sql
   SELECT o.OrderID, c.CustomerName, p.ProductName 
     FROM Orders o 
    INNER JOIN Customers c ON (o.CustomerID = c.CustomerID) 
     INNER JOIN Products p ON (o.OrderID = p.OrderID);
   ```

   

   #### 結果：

   

   | OrderID | CustomerName | ProductName | 
   |---|---|---|
   | 1 | Alice | Laptop | 
   | 1 | Alice | Mouse | 
   | 2 | Bob | Keyboard | 
   | 3 | NULL | Monitor | 
   

   **解釋**：

   - 匹配到 `Orders` 和 `Customers` 表的 `CustomerID`，再匹配到 `Products` 表的 `OrderID`。

   - `OrderID=4` 沒有產品，`CustomerID=103` 也不在顧客表中，所以被排除。

   ---

   ### **2\. LEFT JOIN**

   #### 查詢語句：

   ```sql
   SELECT o.OrderID, c.CustomerName, p.ProductName 
     FROM Orders o 
   LEFT JOIN Customers c ON (o.CustomerID = c.CustomerID) 
   LEFT JOIN Products p ON (o.OrderID = p.OrderID);
   ```

   

   

   #### 結果：

   

   | OrderID | CustomerName | ProductName | 
   |---|---|---|
   | 1 | Alice | Laptop | 
   | 1 | Alice | Mouse | 
   | 2 | Bob | Keyboard | 
   | 3 | NULL | Monitor | 
   | 4 | NULL | NULL | 
   

   **解釋**：

   - `LEFT JOIN` 保留了 `Orders` 表的所有行。

   - 沒有匹配到的數據用 `NULL` 填充，如 `OrderID=4` 和 `CustomerID=103`。

   ---

   ### **3\. RIGHT JOIN**

   #### 查詢語句：

   ```sql
   SELECT o.OrderID, c.CustomerName, p.ProductName 
     FROM Orders o 
    RIGHT JOIN Customers c ON (o.CustomerID = c.CustomerID) 
    RIGHT JOIN Products p ON (o.OrderID = p.OrderID);
   ```

   

   

   #### 結果：

   

   | OrderID | CustomerName | ProductName | 
   |---|---|---|
   | 1 | Alice | Laptop | 
   | 1 | Alice | Mouse | 
   | 2 | Bob | Keyboard | 
   | 3 | NULL | Monitor | 
   | NULL | NULL | Phone | 
   

   **解釋**：

   - `RIGHT JOIN` 保留 `Customers` 和 `Products` 表的所有數據。

   - `OrderID=5` 的產品 `Phone` 被保留，但它的訂單在 `Orders` 表中找不到。

   ---

   ### **4\. FULL JOIN**

   #### 查詢語句：

   ```sql
   SELECT o.OrderID, c.CustomerName, p.ProductName 
     FROM Orders o 
    FULL JOIN Customers c ON (o.CustomerID = c.CustomerID) 
    FULL JOIN Products p ON (o.OrderID = p.OrderID);
   ```

   

   

   #### 結果：

   

   | OrderID | CustomerName | ProductName | 
   |---|---|---|
   | 1 | Alice | Laptop | 
   | 1 | Alice | Mouse | 
   | 2 | Bob | Keyboard | 
   | 3 | NULL | Monitor | 
   | 4 | NULL | NULL | 
   | NULL | Charlie | NULL | 
   | NULL | NULL | Phone | 
   

   **解釋**：

   - `FULL JOIN` 保留所有表的數據。

   - 沒有匹配的行用 `NULL` 填充，例如 `OrderID=4` 沒有客戶和產品，`CustomerID=105` 沒有訂單。

   ---

   ### **5\. CROSS JOIN**

   #### 查詢語句：

   ```sql
   SELECT o.OrderID, c.CustomerName, p.ProductName 
     FROM Orders o 
    CROSS JOIN Customers c 
    CROSS JOIN Products p;
   ```

   

   

   #### 結果：

   

   | OrderID | CustomerName | ProductName | 
   |---|---|---|
   | 1 | Alice | Laptop | 
   | 1 | Alice | Mouse | 
   | 1 | Alice | Keyboard | 
   | ... | ... | ... | 
   

   **解釋**：

   - **笛卡爾積**：所有可能的組合都會生成，結果行數是所有表行數的乘積。

   - 如果 `Orders` 表有 4 行，`Customers` 表有 3 行，`Products` 表有 5 行，最終會生成 `4 x 3 x 5 = 60` 行。

   ---

   ### **結論**

   - **`INNER JOIN`** ：僅保留三表中所有條件都匹配的行。

   - **`LEFT JOIN`** ：保留左表的所有行，即使沒有匹配數據。

   - **`RIGHT JOIN`** ：保留右表的所有行，即使沒有匹配數據。

   - **`FULL JOIN`** ：保留左右表的所有行，沒有匹配的用 `NULL` 填充。

   - **`CROSS JOIN`** ：生成表間所有可能的組合，不建議隨意使用，數據量大時會爆炸。



+ ## 各種JOIN適用場景

   ### 1\. **INNER JOIN**

   **使用場景：**

   - 僅獲取兩個表中有對應關聯記錄的數據。

   - 適用於絕大部分需要匹配主外鍵的查詢。

   **案例：**

   - 查詢訂單詳細資訊以及對應的產品名稱和價格。

   

   

   ### 2\. **LEFT JOIN (或 LEFT OUTER JOIN)**

   **使用場景：**

   - 查詢一個表的所有記錄，即使在另一個表中找不到匹配。

   - 常用於查詢可能沒有關聯資料的情況，例如顯示所有產品即使沒有被訂購。

   **案例：**

   - 查詢所有產品以及它們的銷售數量（即使沒有銷售的產品）。

   

   

   ### 3\. **RIGHT JOIN (或 RIGHT OUTER JOIN)**

   **使用場景：**

   - 查詢一個表的所有記錄，即使在另一個表中找不到匹配。

   - **`不常見`**，通常可以用 LEFT JOIN 改寫。

   **案例：**

   - 查詢所有訂單以及是否有對應的客戶（即使某些訂單沒有對應的客戶）。

   

   

   ### 4\. **FULL JOIN (或 FULL OUTER JOIN)**

   **使用場景：**

   - 查詢兩個表中的所有記錄，即使某些記錄在另一個表中找不到匹配。

   - 用於需要完整數據的場景。

   **案例：**

   - 查詢所有產品和訂單，顯示關聯的訂單數量，並標示出哪些產品或訂單未匹配。

   

   ### **5\. CROSS JOIN**

   **使用場景：**

   - 生成笛卡兒積，用於計算每個元素和每個元素的組合，或者模擬測試數據。

   - 很少在電商中使用，除非需要列出所有可能的組合。

   **案例：**

   - 顯示所有產品和倉庫的組合。

   

   

   ### **6\. SELF JOIN**

   **使用場景：**

   - 一張表內的數據需要相互比較。

   - 電商中常用於層級結構（例如分類樹）。

   **案例：**

   - 查詢產品分類中的父子關係。

   

   

   ### 實際應用案例：

   #### **1\. 訂單報表**

   查詢用戶的訂單以及相關產品資訊：

   ```sql
   SELECT u.UserName, o.OrderID, p.ProductName, o.Quantity, o.TotalPrice 
     FROM Users u 
    INNER JOIN Orders o ON (u.UserID = o.UserID) 
    INNER JOIN Products p ON (o.ProductID = p.ProductID) 
    WHERE u.UserID = 123;
   ```

   

   

   #### **2\. 用戶行為分析**

   查詢用戶瀏覽的產品以及是否已加入購物車：

   ```sql
   SELECT u.UserName, p.ProductName, c.CartID 
     FROM Users u 
    LEFT JOIN ProductViews v ON (u.UserID = v.UserID) 
    LEFT JOIN CartItems c ON (v.ProductID = c.ProductID) 
    WHERE u.UserID = 123;
   ```

   

   

   #### **3\. 銷售統計**

   統計每個產品的銷售情況：

   ```sql
   SELECT p.ProductName, SUM(o.Quantity) AS TotalSold 
     FROM Products p 
   LEFT JOIN Orders o ON (p.ProductID = o.ProductID) 
   GROUP BY p.ProductName;
   ```

   

   

   



+ ## 使用join時，select的表名怎麼確定?有些是左表的欄位，一些是右表的

   1. **表別名（Alias，強烈推薦）**：

   使用簡短的表別名，方便書寫並提高可讀性。

   別名是為表取的暫時代號，格式為 `表名 AS 別名`（`AS` 可省略）。

   範例：

   ```sql
   SELECT o.OrderID, c.CustomerName 
   FROM Orders AS o JOIN Customers AS c ON o.CustomerID = c.CustomerID;
   ```

   

   2. **避免重複欄位名稱**：

   可以為查詢結果中的欄位取別名，方便前端或應用層處理。

   範例：

   ```sql
   SELECT o.CustomerID AS OrderCustomerID, c.CustomerName AS CustomerName 
   FROM Orders o 
   JOIN Customers c ON o.CustomerID = c.CustomerID;
   ```

   

   

   3. **跨多表時**：

   使用清晰的別名，能快速區分數據來源。

   範例（跨三表查詢）：

   ```sql
   SELECT o.OrderID, c.CustomerName, p.ProductName 
   FROM Orders o 
   JOIN Customers c ON o.CustomerID = c.CustomerID 
   JOIN Products p ON o.OrderID = p.OrderID;
   ```

   

   



+ ## 怎麼判斷題目哪些是放在左表?哪些是放在右表

   基於業務邏輯判斷

   在多數情況下，你可以根據業務需求判斷左表與右表：

   1. **左表通常是主要數據來源**：

      - 左表包含你想主要查詢的數據。

      - 右表提供輔助數據（如附加資訊或過濾條件）。

   #### 範例 1：查詢比賽中的進球者

   ```sql
   SELECT game.team1, game.team2, goal.player 
   FROM game 
   JOIN goal ON game.id = goal.matchid;
   ```

   

   - 左表：`game`，因為你查詢的是比賽相關的資訊（`team1` 和 `team2`）。

   - 右表：`goal`，因為你需要輔助數據（進球者）。

   ### **2\. JOIN 的不同類型對左右表的影響**

   #### **INNER JOIN**

   - 左右表的角色對結果影響不大，因為它只返回匹配的數據。

   #### **LEFT JOIN**

   - 左表的數據會全部保留，即使右表中沒有匹配。

   - 如果某些數據在右表中找不到，結果中的右表欄位會顯示 `NULL`。

   範例：

   ```sql
   SELECT game.team1, game.team2, goal.player 
   FROM game 
   LEFT JOIN goal ON game.id = goal.matchid;
   ```

   

   - 目的是保留所有比賽（左表）的數據，即使某些比賽中沒有進球（右表）。

   #### **RIGHT JOIN**

   - 與 `LEFT JOIN` 相反，右表的數據會全部保留。

   #### **FULL JOIN**

   - 所有左右表的數據都保留。

   ### **總結步驟**

   判斷哪些是左表、哪些是右表的清晰方法：

   1. **查詢語句結構**：`FROM` 後的表是左表，`JOIN` 後的表是右表。

   2. **欄位對應**：通過 `SELECT` 或 `ON` 條件來確認每個欄位的來源。

   3. **業務邏輯**：根據需求確定主表（通常作為左表）和輔表（通常作為右表）。

   4. **JOIN 類型**：考慮是否需要保留左表或右表的所有數據，決定使用哪種 JOIN。

   

   



+ ## ON 有哪些用法

   SQL 中的 **`ON`** 是 `JOIN` 子句的一部分，用於指定兩個表之間的連接條件或匹配邏輯。`ON` 是 `JOIN` 的核心，決定了如何將多個表中的記錄組合在一起。

   

   ### 懶人包

   - 基本用法：表連接 (inner join)

   - 多條件匹配  (後面再指定 AND 或是 OR)

   - 非等值連接 (Non-Equi Join)  (between 或是 > <)

   - 自連接 (Self Join)

   - 外部連接 (LEFT/RIGHT JOIN)

   - 跨連接 (CROSS JOIN)

   - 使用 `USING` 替代 `ON`

   - 自然連接 (NATURAL JOIN)

   

   

   以下是 **`ON`** 在不同情況下的主要用法和範例：

   ---

   ### 1\. **基本用法：表連接**

   在 `JOIN` 中使用 `ON` 指定匹配條件。

   #### 範例：

   假設有兩個表：

   **Employees**

   

   | employee_id | name | department_id | 
   |---|---|---|
   | 1 | Alice | 101 | 
   | 2 | Bob | 102 | 
   | 3 | Charlie | 103 | 
   

   **Departments**

   

   | department_id | department_name | 
   |---|---|
   | 101 | HR | 
   | 102 | IT | 
   | 104 | Finance | 
   

   **查詢：**

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   JOIN Departments d ON e.department_id = d.department_id;
   ```


   **結果：**

   

   | name | department_name | 
   |---|---|
   | Alice | HR | 
   | Bob | IT | 
   

   ---

   ### 2\. **多條件匹配**

   `ON` 支持多個條件，可以使用 `AND` 或 `OR` 組合。

   #### 範例：

   匹配部門 ID 並且員工的名字是 "Alice"。

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   JOIN Departments d ON e.department_id = d.department_id AND `[`e.name`](e.name)` = 'Alice';
   ```


   ---

   ### 3\. **非等值連接 (Non-Equi Join)**

   `ON` 不僅支持等值匹配，還支持其他操作符，例如 `<`, `>`, `<=`, `BETWEEN` 等。

   #### 範例：

   匹配員工的 `department_id` 小於部門表的 `department_id`。

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   JOIN Departments d ON e.department_id < d.department_id;
   ```

   ---

   ### 4\. **自連接 (Self Join)**

   `ON` 用於同一個表的連接，通過指定不同的別名。

   #### 範例：

   找出每個員工的上司（假設上司的 `employee_id` 存在於員工的 `manager_id` 欄位）。

   ```sql
   SELECT `[`e.name`](e.name)` AS employee, `[`m.name`](m.name)` AS manager 
   FROM Employees e 
   JOIN Employees m ON e.manager_id = m.employee_id;
   ```


   ---

   ### 5\. **外部連接 (LEFT/RIGHT JOIN)**

   `ON` 指定連接條件，並返回符合條件的記錄以及非匹配部分。

   #### 範例：

   列出所有員工及其部門名稱，若部門不存在，則部門名稱為 `NULL`。

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   LEFT JOIN Departments d ON e.department_id = d.department_id;
   ```


   **結果：**

   

   | name | department_name | 
   |---|---|
   | Alice | HR | 
   | Bob | IT | 
   | Charlie | NULL | 
   

   ---

   ### 6\. **跨連接 (CROSS JOIN)**

   `ON` 不用於 `CROSS JOIN`，因為它返回兩個表的笛卡爾積（所有可能的組合）。

   #### 範例：

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   CROSS JOIN Departments d;
   ```


   **結果：**

   

   | name | department_name | 
   |---|---|
   | Alice | HR | 
   | Alice | IT | 
   | Alice | Finance | 
   | Bob | HR | 
   | Bob | IT | 
   | Bob | Finance | 
   | Charlie | HR | 
   | Charlie | IT | 
   | Charlie | Finance | 
   

   ---

   ### 7\. **使用 `USING` 替代 `ON`**

   如果兩個表有相同名稱的欄位，可以使用 `USING` 替代 `ON`。

   #### 範例：

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   JOIN Departments d USING (department_id);
   ```


   相當於：

   ```sql
   ON e.department_id = d.department_id
   ```


   ---

   ### 8\. **自然連接 (NATURAL JOIN)**

   `ON` 不用於 `NATURAL JOIN`，因為它自動基於同名欄位匹配。

   #### 範例：

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e NATURAL JOIN Departments d;
   ```

   ---

   ### 總結

   **`ON` 的主要功能：**

   1. 明確定義連接條件。

   2. 支持多條件匹配、非等值連接、自連接。

   3. 與 `LEFT JOIN`、`RIGHT JOIN` 等結合處理非匹配記錄。

   4. `ON` 需要具體條件，而 `NATURAL JOIN` 和 `CROSS JOIN` 不需要。



+ ## ON 多條件匹配 如果不符合條件的會怎樣

   在 SQL 的 **多條件匹配** 中，`ON` 子句指定的條件控制兩個表的記錄是否能夠連接。如果記錄不符合條件，結果取決於使用的 **連接類型**（`INNER JOIN`、`LEFT JOIN`、`RIGHT JOIN` 等）。以下是不同情況下的行為：

   ---

   ### **1\. INNER JOIN（內連接）**

   - **行為：**

      - 如果兩個表中的記錄不符合 `ON` 條件，這些記錄將被過濾掉，不會出現在最終結果中。

   #### 範例：

   ```sql
    SELECT `[`e.name`](e.name)`, d.department_name 
    FROM Employees e 
    INNER JOIN Departments d ON e.department_id = d.department_id AND `[`e.name`](e.name)` = 'Alice';
   ```


   **假設數據如下：**

   **Employees**

   

   | employee_id | name | department_id | 
   |---|---|---|
   | 1 | Alice | 101 | 
   | 2 | Bob | 102 | 
   

   **Departments**

   

   | department_id | department_name | 
   |---|---|
   | 101 | HR | 
   | 102 | IT | 
   

   **結果：**

   

   | name | department_name | 
   |---|---|
   | Alice | HR | 
   

   **說明：**

   - 僅有 `Alice` 符合 `name = 'Alice'`，因此其他員工被過濾掉。

   ---

   ### **2\. LEFT JOIN（左連接）**

   - **行為：**

      - 如果左表的記錄不符合 `ON` 條件，仍會顯示該記錄，但右表的值設為 `NULL`。

   #### 範例：

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   LEFT JOIN Departments d ON e.department_id = d.department_id AND `[`e.name`](e.name)` = 'Alice';
   ```


   **結果：**

   

   | name | department_name | 
   |---|---|
   | Alice | HR | 
   | Bob | NULL | 
   

   **說明：**

   - `Alice` 符合條件，所以她的部門名稱被顯示。

   - `Bob` 不符合 `name = 'Alice'`，因此他的部門名稱是 `NULL`。

   ---

   ### **3\. RIGHT JOIN（右連接）**

   - **行為：**

      - 如果右表的記錄不符合 `ON` 條件，仍會顯示該記錄，但左表的值設為 `NULL`。

   #### 範例：

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   RIGHT JOIN Departments d ON e.department_id = d.department_id AND `[`e.name`](e.name)` = 'Alice';
   ```


   **結果：**

   

   | name | department_name | 
   |---|---|
   | Alice | HR | 
   | NULL | IT | 
   

   **說明：**

   - `Alice` 符合條件，部門名稱為 `HR`。

   - `IT` 部門未能匹配其他員工，因此 `name` 顯示為 `NULL`。

   ---

   ### **4\. FULL OUTER JOIN（全外連接）**

   - **行為：**

      - 如果左右表中的記錄都不符合 `ON` 條件，兩邊的值都設為 `NULL`。

   #### 範例：

   ```sql
   SELECT `[`e.name`](e.name)`, d.department_name 
   FROM Employees e 
   FULL OUTER JOIN Departments d ON e.department_id = d.department_id AND `[`e.name`](e.name)` = 'Alice';
   ```


   **結果：**

   

   | name | department_name | 
   |---|---|
   | Alice | HR | 
   | Bob | NULL | 
   | NULL | IT | 
   

   **說明：**

   - `Alice` 符合條件，因此顯示她和她的部門。

   - `Bob` 沒有匹配到部門，因此部門為 `NULL`。

   - `IT` 部門沒有匹配到員工，因此 `name` 顯示為 `NULL`。

   ---

   ### **5\. CROSS JOIN（笛卡爾積，無條件）**

   - **行為：**

      - `CROSS JOIN` 不使用 `ON` 條件，每條記錄與另一個表的每條記錄組合，因此與 `ON` 無關。

   ---

   ### **總結：多條件匹配的影響**

   - **條件不符合的記錄處理方式**完全取決於連接類型：

      1. **`INNER JOIN`** ：過濾掉不匹配的記錄。

      2. **`LEFT JOIN`** ：保留左表不匹配的記錄，右表設為 `NULL`。

      3. **`RIGHT JOIN`** ：保留右表不匹配的記錄，左表設為 `NULL`。

      4. **`FULL OUTER JOIN`** ：保留左右表的所有記錄，未匹配的設為 `NULL`。