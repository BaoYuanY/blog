---
title: Linux安装Docker
date: 2023-06-12 10:17:00
tags: ['docker']
categories: ['Linux']
excerpt: "依赖CentOS 7.6"
---

## 安装额外的工具

``` bash
yum install -y yum-utils \
           device-mapper-persistent-data \
           lvm2 --skip-broken
```


## 一. 如果之前安装过Docker，可以使用下面命令卸载

```bash
    yum remove docker \
                      docker-client \
                      docker-client-latest \
                      docker-common \
                      docker-latest \
                      docker-latest-logrotate \
                      docker-logrotate \
                      docker-selinux \
                      docker-engine-selinux \
                      docker-engine \
                      docker-ce
```

## 二. 设置Docker镜像源

```bash
    yum-config-manager \
        --add-repo \
        https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

```bash
sed -i 's/download.docker.com/mirrors.aliyun.com\/docker-ce/g' /etc/yum.repos.d/docker-ce.repo
```

```bash
yum makecache fast
```


## 三. 安装
```bash
yum install -y docker-ce
```


## 四. 启动、停止、重启docker命令

```bash
# 启动docker服务
systemctl start docker  

# 停止docker服务
systemctl stop docker  

# 重启docker服务
systemctl restart docker  
```


输入 `systemctl status docker` 查看docker启动状态 如下图

![image](/images/linux-insert-docker/docker-status.png)

输入 `docker -v` 查看docker版本

![image](/images/linux-insert-docker/docker-version.png)

## 五. 配置镜像加速

可以使用自己的阿里云账号配置(没有账号的话建议注册一下) 详情查看文档 ===> <a herf="https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors">容器镜像服务</a>

![image](/images/linux-insert-docker/docker-mirrors.png)

### 配置说明

```bash
# sudo前缀针对非root用户且有sudo权限的用户 视情况加与不加

# 创建文件夹
sudo mkdir -p /etc/docker

# 将配置写入文件
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://7pvmk0yu.mirror.aliyuncs.com"]
}
EOF

# 重新加载文件
sudo systemctl daemon-reload

# 重启docker
sudo systemctl restart docker
```

## PS:

如果遇到一些问题 可以试着关闭防火墙后重试

```bash
# 关闭
systemctl stop firewalld

# 禁止开机启动防火墙
systemctl disable firewalld

#查看是否关闭防火墙
systemctl status firewalld
```






