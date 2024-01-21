---
title: 使用Docker 安裝 MySQL
date: 2024-1-18 11:38
categories: MySQL
tags:
    - MySQL
    - Docker
---

# 前言
本文介紹使用 Docker 安裝 MySQL，Docker 是安裝在本機 wsl 裡面，docker 安裝過程就不贅述。

# 確認wsl 中 Docker 是否有開啟

`service docker status`

啟動docker
`service docker start`

# 拉取MySQL 映像
`docker pull mysql:8`

# 檢查 image 是否有下載成功
`docker images`

# 運行 MySQL 容器
`docker run --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=YourRootPassword -d mysql:8`

* `--name mysql-test` : 指定容器為 mysql-test
* `-p 3306:3306`(host port:container port)  : 將主機的 3306 端口映射到容器的 3306 端口，mysql 預設啟動時是跑3306 port，設定此port 比較省事
* `-e MYSQL_ROOT_PASSWORD=YourRootPassword` : 初始化 root 用戶的密碼為 YourRootPassword。
* `-d mysql:8` : 背景執行 MySQL 映像

# 檢查運行中的容器
`docker ps`

# 進入容器
`docker exec -it mysql-test bash`


# 進入容器後，登陸MySQL
`mysql -u root -p`

# 在wsl 中開啟兩個docker container ( MySQL)

有時候需要會同時需要開啟多個mysql，最簡單的方式就是容器映射不同的port

## 方法1 (調整容器的port)
容器1
`docker run --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=YourRootPassword -d mysql:8`

容器2 (將容器內3306 port 映射到主機 3307port)
`docker run --name mysql-test -p 3307:3306 -e MYSQL_ROOT_PASSWORD=YourRootPassword -d mysql:8`

最省事，也不用修改啥配置文件

## 方法2 (調整mysql 配置文件)

詳見`MySQL修改配置文件`內容

# MySQL 

每個語句後面要用 `;` 結尾，不然 MySQL 會以為你還沒輸入完，出現 `->` 記號

## 修改root 密碼
``` SQL
ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourNewPassword';
```

* localhost 代表允許從哪個主機遠端連線，也可以設置為 `%`， 代表允許從任何主機遠端連線


## 添加用戶

遠端連線時，不要使用root 用戶比較好，因此額外新增一個用戶

設定用戶時可以指定用戶使用語句的權限(SELECT、INSERT...)

以及對那些表有訪問的權限

## 建立用戶
``` SQL
CREATE USER 'your_user_name'@'%' IDENTIFIED WITH mysql_native_password BY 'your_password';
```

* `your_username` 為你要創建的用戶名
* `your_password` 為你要設置的密碼

## 給予用戶權限

### 給予所有權限
``` SQL
GRANT ALL PRIVILEGES ON *.* TO 'your_user_name'@'%';
```


### 指給予某個權限
``` SQL
GRANT SELECT ON tableName.* TO 'your_username'@'%';
```



* GRANT SELECT： 表示要給予使用者 SELECT 權限。
* ON transaction.*： 表示你要將許可權應用到 transaction 資料庫的所有表。 * 表示所有表。
* TO 'your_username'@'%'： 指定了使用者和來源主機。 'your_username' 是你要授予許可權的使用者名，'%' 表示允許從任何主機遠端登錄。
這樣，使用者 『your_username』 將只有在 transaction 資料庫的所有表上執行 SELECT 操作的許可權。


## 建立資料庫
``` SQL
create database yourDbName;
```

顯示資料庫
``` SQL
show databases;
```


選擇資料庫
``` SQL
use yourDbName;
```


## 建立資料表

看語意就知道如果資料表存在就刪除，確定要刪除才執行，否則後果很嚴重

``` SQL
drop table if exists tableName;
```


建立新的資料表
``` SQL
create table tableName (
    id int auto_increment primary key,
    name nvarchar(30),
    version nvarchar(30),
    remark nvarchar(30)
);
```


## 插入資料

``` SQL
insert into tableName(name,version,remark) values
('sisi','1.05','cool man')
('coco','0.7','poor guy');
```



# 修改配置文件
一般來說不需要，除非有例如更換服務port的需求

進入配置文件的目錄
`cd /etc/mysql/conf.d`

新增一個mysql.cnf
`touch mysql.cnf`

以下都要寫入
```
[mysql]
port = 3307
```

修改完配置重新啟動容器
`docker reatart mysql-test`


## 離開MySQL

`quit` 或是 `exit` 都行

# 連線

從外部連線MySQL 需要連線字串，主機名因為MySQL 是安裝在wsl 的 Docker 裡面，只要在wsl 獲取ip後，就能透過連線字串連接

在wsl 終端
`ifconfig`

找到 `eth0` 字段的ip 值

例如ip為
172.27.119.190

連線字串
`server=172.27.119.190;port=3306;user id=your-user-name;password=your-user-password;database=database-name;charset=utf8;`


# 參考
[【Docker 攻略】MySQL 安裝篇 | 安裝資料庫 好快的說 !](https://ithelp.ithome.com.tw/articles/10272193)