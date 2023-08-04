---
title: Linux+clash
date: 2023-08-04 23:43
tags: ['clash']
categories: ['Linux', 'clash']
excerpt: "在Linux中开启代理clash"
---

{% notel blue 环境 %}
**SERVER**: CentOS 7.6
{% endnotel %}

## 1. 更新系统及安装必要的包
```shell
sudo yum update -y
sudo yum install -y wget unzip
```

## 2. 准备clash二进制文件
{% btn regular::跳转Github/Clash::https://github.com/Dreamacro/clash/releases::fa-brands fa-github %}
例如我下载的是下图中框起来的
![image](https://qiniu.yangbaoyuan.cn/assets.png)

毕竟是要在服务器上安装clash，所以推荐步骤是先下载到本地，然后使用SFTP上传到服务器，然后在服务器上解压
如果你想直接下载的话也可以使用 
`wget https://github.com/Dreamacro/clash/releases/download/v1.17.0/clash-linux-amd64-v1.17.0.gz`
如果你不知道链接是什么，可以把鼠标放在对应包的上面，然后看浏览器的左下角（如果左下角没有就查看网页源代码）
![image](https://qiniu.yangbaoyuan.cn/clashWgetUrl.png)

下载到服务器上以后放在 `/usr/local/bin` 目录下解压然后重命名
```shell
#解压
gunzip clash-linux-amd64-v1.17.0.gz
#重命名
mv clash-linux-amd64-v1.17.0 clash
```




