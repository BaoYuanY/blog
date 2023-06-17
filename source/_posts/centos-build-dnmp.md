---
title: Linux使用Docker搭建PHP开发环境
date: 2023-06-15 00:00:00
tags: ['dnmp']
categories: ['Docker', 'PHP']
excerpt: "在Docker中搭建dnmp(PHP开发环境)"
---

# 一. Docker安装

{% btn regular::Linux安装Docker::/linux-insert-docker::fa-solid fa-newspaper %}

# 二. 安装 `docker-compose`

### 1. 下载
```shell
sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

#慢的话可以用这个
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

### 2. 设置权限
```shell
sudo chmod +x /usr/local/bin/docker-compose
```

### 3. 验证
```shell
docker-compose --version
```

# 三. 拉取dnmp环境配置

按需拉取 随便放在一个地方

{% tabs 代码克隆 %}
<!-- tab gitee-->

```shell
git clone https://gitee.com/baoyuan0304/dnmp.git
```

<!-- endtab -->

<!-- tab github-->

```shell
git clone -b Linux https://github.com/BaoYuanY/dnmp.git
```

<!-- endtab -->

<!-- tab gitlab-->

```shell
git clone -b Linux https://gitlab.yangbaoyuan.cn/BaoYuan/dnmp.git
```

<!-- endtab -->
{% endtabs %}

# 四. 配置

```shell
mkdir /www              #设置挂载目录
cd /www            
git clone ******        #克隆dnmp代码 
vim .env                #编辑 .env
#修改 SOURCE_DIR=/www  
```
在dnmp目录下执行
```shell
docker-compose up -d  #第一次会先build 时间稍微长一点
```


<b>然后配置自己的nginx的*****.conf文件</b>































