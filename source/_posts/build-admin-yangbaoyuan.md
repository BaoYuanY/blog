---
title: Laravel
date: 2023-06-19 23:26:00
tags: [ 'Laravel' ]
categories: [ 'PHP' , 'Laravel' ]
excerpt: "记录admin.yangbaoyuan.cn搭建"
---

{% notel blue 环境配置 %}
Server: `CentOS Linux release 7.9.2009 (Core)`
Docker: `24.0.4`
Docker-compose: `1.29.2`
PHP: `7.4.27`
Composer: `2.5.8`
Laravel: `8.5`
Redis: `6.2.6`
Mysql: `8.0.27`
{% endnotel %}
## admin.yangbaoyuan.cn
项目地址:  {% btn regular::Gitlab::https://gitlab.yangbaoyuan.cn/BaoYuan/admin.yangbaoyuan::fa-brands fa-gitlab %}
### migration 相关
#### 添加表备注
```php
//添加到对应的migration文件中
\Illuminate\Support\Facades\DB::statement("ALTER TABLE `table` COMMENT '********'");
```
### 权限相关
#### 1. docker内权限配置
```shell
chown -hR 1000:1000 /www
```




