---
title: MySQL一些语句汇总
date: 2023-06-25 23:27:00
tags: [ 'MySQL' ]
categories: [ 'MySQL' ]
excerpt: "总结..."
---


# 查询数据库中一个或两个字段重复的

```sql
SELECT field1, field2, COUNT(*) as count
FROM `table`
GROUP BY field1, field2
HAVING count > 1;    
```


# 截断某张表

```sql
truncate `table`;
```