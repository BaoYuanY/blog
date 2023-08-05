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
sudo yum install -y git
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

## 3. 配置配置文件
{% btn regular::配置说明文档::https://dreamacro.github.io/clash/zh_CN/configuration/getting-started.html::fa-brands fa-github %}
![image](https://qiniu.yangbaoyuan.cn/clashConfigDocument.png)

```shell
#依次使用如下命令
cd ~
mkdir -p .config/clash

#第一种 wget方式 （我的订阅地址下载下来并不直接是配置文件，所以会用第二种方式）
wget -O config.yaml {订阅地址}

#第二种 用本地的配置文件复制到服务器上
...
```
配置结果如下图
![image](https://qiniu.yangbaoyuan.cn/clashConfigYaml.png)
需要在原有的基础上修改和添加一些东西
![image](https://qiniu.yangbaoyuan.cn/clashYamlContent.png)
1. 修改 `external-controller` 为 **`0.0.0.0:9090`** (把IP限制放开)
2. 添加 `external-ui` 为 **`/usr/local/bin/clash-dashboard`** (配置UI界面的路径)
3. 添加 `secret` (登录密码，可自由设定)

## 4. 配置UI界面
{% btn regular::跳转Clash-Dashboard::https://github.com/Dreamacro/clash-dashboard::fa-brands fa-github %}

```shell
#进入配置文件对应的目录
cd /usr/local/bin/

#克隆 gh-pages (已经编译好的html)
git clone -b gh-pages https://github.com/Dreamacro/clash-dashboard.git

```
## 5. 使用clash
```shell
cd /usr/local/bin

#开启
./clash
```
![image](https://qiniu.yangbaoyuan.cn/clashCompile.png)

## 6. UI
1. 确保服务器端口`9090`是允许访问的
2. 在浏览器输入 **IP:9090/ui** （IP填写你自己的IP地址）
![image](https://qiniu.yangbaoyuan.cn/clashUi.png)


## 7. 设置系统管理
```shell
vim /etc/systemd/system/clash.service
```
将下列内容粘贴到文件内
```plantuml
[Unit]
Description=Clash daemon, A rule-based proxy in Go.
After=network.target

[Service]
Type=simple
Restart=always
ExecStart=/usr/local/bin/clash -f /root/.config/clash/config.yaml

[Install]
WantedBy=multi-user.target
```


```shell
#重新加载配置文件
systemctl daemon-reload

#开启clash (要么再第五步开启clash,要么使用systemctl开启,开一处即可)
systemctl start clash

#重启clash
systemctl restart clash

#关闭clash
systemctl stop clash

#查看clash状态
systemctl status clash

#开机自启
systemctl enable clash
```
![image](https://qiniu.yangbaoyuan.cn/clashStatus.png)

## 9. 设置系统代理
```shell
cd ~
vim .bash_profile
```
将函数添加在文件中
```shell
function proxy_off() {
    # 移除代理
    unset http_proxy
    unset https_proxy
    unset all_proxy
    echo '********   关闭当前终端代理   ********'
}

function proxy_on() {
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7891
    echo '********   开启当前终端代理   ********'
}
```

![image](https://qiniu.yangbaoyuan.cn/clashProxyOnOrOff.png)








