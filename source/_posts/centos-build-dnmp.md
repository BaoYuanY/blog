---
title: Linux使用Docker搭建PHP开发环境
date: 2023-06-15 00:00:00
tags: ['dnmp']
categories: ['Docker', 'PHP']
excerpt: "在Docker中搭建dnmp(PHP开发环境)"
---

# 1. Docker安装

我直接使用的是yum安装

```shell
##安装docker
yum install docker

##安装docker-compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

结果如图所市

![目录示例](/images/centos-build-dnmp/dnmp-dir.png)


# 2. 直接拉环境配置

想好放在哪个目录后使用git克隆代码

```shell
git clone -b Linux https://github.com/BaoYuanY/dnmp.git
```

如下

![目录示例](/images/centos-build-dnmp/dnmp-dir.png)
我放在了用户主目录下。其中：
- dnmp是搭建的php开发环境
- wwwroot是我挂载的目录

所以我们在配置dnmp的env的时候 `SOURCE_DIR` 就要是`/home/yby/wwwroot`
如果不知道后面写什么 可以参照如下图所示

![目录示例](/images/centos-build-dnmp/dnmp-source-dir.png)

拿到路径以后填充到对应的配置

![目录示例](/images/centos-build-dnmp/set-source-dir.png)












































