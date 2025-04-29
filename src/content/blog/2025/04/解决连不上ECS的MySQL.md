---
title: "解决连不上ECS的MySQL"
categories: question
tags: ['MySQL远程访问权限设置']
id: "01d96a867053a314"
date: 2025-04-29 14:17:46
cover: "https://wp-cdn.4ce.cn/v2/nVsP0zq.jpeg"
---

:::note
解决连不上ECS的MySQL
:::

**1. MySQL 异常: "Host 'xxx' is not allowed to connect to this MySQL server"**？

```mysql
# 使用数据库mysql
use mysql;
# 查看主机和用户
mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
4 rows in set (0.00 sec)
```

```mysql
mysql> update user set host = '%' where user = 'root';
```

```mysql
mysql> FLUSH PRIVILEGES; 
回车使刚才的修改生效，再次远程连接数据库成功。
```

