---
title: MySQL一些语句汇总
date: 2023-06-25 23:27:00
tags: [ 'MySQL' ]
categories: [ 'MySQL' ]
excerpt: "总结..."
---


# ‣ 查询数据库中一个或两个字段重复的

```sql
SELECT field1, field2, COUNT(*) as count
FROM `table`
GROUP BY field1, field2
HAVING count > 1;    
```


# ‣ 截断某张表

```sql
truncate `table`;
```

# ‣ 新增用户
要在MySQL中创建一个新的管理员用户并允许所有IP地址登录，您可以按照以下步骤操作：

1. 首先，用已有的管理员账户（例如，'root'）登录到MySQL命令行，输入以下命令：
```
mysql -u root -p
```
然后输入您的密码。

2. 在MySQL命令行中，运行以下命令以创建一个新用户（例如，'new_admin'）并设置密码（将'your_password'替换为您真正的密码）：
```sql
CREATE USER 'new_admin'@'%' IDENTIFIED BY 'your_password';
```
`'%'`意味着允许来自任意IP地址的连接。

3. 现在，要赋予新用户所有数据库的所有权限，运行以下命令：
```sql
GRANT ALL PRIVILEGES ON *.* TO 'new_admin'@'%' WITH GRANT OPTION;
```
这将为新用户提供执行所有操作的权限，并允许其授予其他用户权限。

4. 使更改生效，执行以下命令：
```sql
FLUSH PRIVILEGES;
```

5. 最后，使用`\q`命令退出MySQL命令行：
```
\q
```

现在，您已成功创建了一个新的管理员用户，并允许从任何IP地址登录。用户可以使用以下命令连接到MySQL服务器（确保使用实际的MySQL服务器地址替换`your_server_address`）：
```
mysql -u new_admin -p -h your_server_address
```
然后输入相应的密码进行登录。


# ‣ 将idb文件转化为sql

1. 断开表：

```sql
USE your_database;
ALTER TABLE `your_table` DISCARD TABLESPACE;
```

2. 然后，导入.ibd文件以及更名后的表名：

```sql
ALTER TABLE `your_table` IMPORT TABLESPACE;
```






















