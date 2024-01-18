---
title: 使用Docker 安裝 MySQL
date: 2024-1-18 11:38
categories: MySQL
tags:
    - MySQL
    - Docker
---

# 前言
本文介紹使用 Docker 安裝 MySQL，Docker 是安裝在本機 wsl 裡面

# 確認wsl 中 Docker 是否有開啟

service docker status

啟動docker
service docker start

# 拉取MySQL 映像
docker pull mysql:8

# 檢查 image 是否有下載成功
docker images

# 運行 MySQL 容器
docker run --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=YourRootPassword -d mysql:8

* --name : 指定容器為 mysql-test
* -p 3306:3306 : 將容器的 3306 端口映射到主機的 3306 端口，mysql 預設啟動時是跑3306 port，設定此port 比較省事
* -e MYSQL_ROOT_PASSWORD=YourRootPassword : 初始化 root 用戶的密碼為 YourRootPassword。
* -d mysql:8 : 背景執行 MySQL 映像

# 檢查運行中的容器
docker ps

# 進入容器
docker exec -it mysql-test bash


# 進入容器後，登陸MySQL
mysql -u root -p




# MySQL 

每個語句後面要用; 結尾，不然MySQL 會以為你還沒輸入完，出現 -> 記號

## 修改root 密碼
ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourNewPassword';


## 添加用戶

遠端連線時，不要使用root 用戶比較好，因使額外新增一個用戶

設定用戶時可以指定用戶使用語句的權限(SELECT、INSERT...)

對那些表有訪問的權限

## 建立用戶
CREATE USER 'your_user_name'@'%' IDENTIFIED WITH mysql_native_password BY 'your_password';

替换 'your_username' 为你要创建的用户名。
替换 'your_password' 为你要设置的密码。

## 給予用戶權限
GRANT ALL PRIVILEGES ON *.* TO 'your_user_name'@'%';

GRANT SELECT ON tableName.* TO 'your_username'@'%';


GRANT SELECT: 表示你要授予用户 SELECT 权限。
ON transaction.*: 表示你要将权限应用到 transaction 数据库的所有表。* 表示所有表。
TO 'your_username'@'%': 指定了用户和来源主机。'your_username' 是你要授予权限的用户名，'%' 表示允许从任何主机远程登录。
这样，用户 'your_username' 将只有在 transaction 数据库的所有表上执行 SELECT 操作的权限。

请确保在授权时谨慎操作，仅授予用户他们实际需要的权限，以提高系统的安全性。

## 建立資料庫
create database yourDbName;

顯示資料庫
show databases;

選擇資料庫
use yourDbName;

## 建立資料表

看語意就知道如果資料表存在就刪除，確定要刪除才執行，否則後果很嚴重
drop table if exists tableName;

建立新的資料表
create table tableName (
    id int auto_increment primary key,
    name nvarchar(30),
    version nvarchar(30),
    remark nvarchar(30)
);

## 插入資料

insert into tableName(name,version,remark)

## 離開MySQL

quit 或是 exit 都行

# 連線

從外部連線MySQL 需要連線字串，主機名因為MySQL 是安裝在wsl 的 Docker 裡面，只要在wsl 獲取ip後，就能透過連線字串連接

ifconfig

eth0

172.27.119.190

連線字串
server=172.27.119.190;port=3306;user id=your-user-name;password=your-user-password;database=database-name;charset=utf8;


# 參考
[【Docker 攻略】MySQL 安裝篇 | 安裝資料庫 好快的說 !](https://ithelp.ithome.com.tw/articles/10272193)