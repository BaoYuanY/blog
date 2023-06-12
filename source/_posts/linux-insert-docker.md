---
title: Linux安装Docker
date: 2023-06-12 10:17:00
tags: ['docker']
categories: ['Linux']
excerpt: "依赖CentOS 7.6"
---

## 首先安装yum的工具

``` shell
yum install -y yum-utils \
           device-mapper-persistent-data \
           lvm2 --skip-broken
```


## 设置docker镜像源











