---
title: CentOS有关SSH配置
date: 2023-05-24 13:53:00
tags: ['SSH']
categories: ['Linux', 'CentOS']
excerpt: "ssh-keygen -t rsa"
---

# SSH的安装

1. 首先，确保已在CentOS服务器上安装了openssh-server。通过运行以下命令检查是否已安装：

   ``` shell
   yum list installed | grep openssh-server
   ```
   如果未安装，请使用以下命令安装：

   ``` shell
   sudo yum install openssh-server
   ```

2. 启动SSH服务并设置为开机自启动：

   ``` shell
   sudo systemctl start sshd
   sudo systemctl enable sshd
   ```

3. 使用防火墙允许SSH连接。防火墙默认情况下会阻止SSH连接，因此需要通过以下命令打开SSH端口：

   ``` shell
   sudo firewall-cmd --permanent --zone=public --add-service=ssh
   sudo firewall-cmd --reload
   ```
   或者，如果要打开某个特定端口，如22，使用以下命令：

   ``` shell
   sudo firewall-cmd --permanent --zone=public --add-port=22/tcp
   sudo firewall-cmd --reload
   ```

4. 编辑SSH配置文件，以添加或修改配置。使用文本编辑器编辑`/etc/ssh/sshd_config`文件：

   ``` shell
   sudo vi /etc/ssh/sshd_config
   ```
   常见的配置选项包括：

    - 禁用ROOT登录：`PermitRootLogin no`。建议禁用Root登录，以增加安全性。
    - 禁止密码登录：`PasswordAuthentication no`。
    - 开始ssh登录：`PubkeyAuthentication yes`。
    - 更改默认端口：将`Port`选项更改为所需端口（如`Port 2222`）。这可以帮助减少暴力攻击。
    - 限制用户访问：例如，要仅允许用户alice和bob登录，需要添加`AllowUsers alice bob`。

   在编辑配置文件后，保存并关闭文件。

5. 重启SSH服务，使更改生效：

   ``` shell
   sudo systemctl restart sshd
   ```

6. 配置客户端。要从客户端登录到SSH服务器，需要在客户端生成SSH密钥（公钥和私钥）。在客户端运行以下命令：

   ``` shell
   ssh-keygen
   ```
   按照提示操作，您将会创建一个新的密钥对。然后将公钥添加到CentOS服务器的`~/.ssh/authorized_keys`文件中：

   ``` shell
   ssh-copy-id user@your_server_ip
   ```
   （将'user'和'your_server_ip'替换为实际的值）

7. 现在您已经配置好SSH登录了。要登录到远程服务器，请从客户端运行以下命令：

   ``` shell
   ssh user@your_server_ip
   ```
   （将'user'和'your_server_ip'替换为实际的值）

就这样，您已经配置并从客户端访问了新的CentOS服务器。保持登录安全！


# ssh空闲超时时间检测

1. 使用文本编辑器（例如vi,nano,vim）编辑配置文件
   ``` shell
   vi /etc/ssh/sshd_config
   ```
2. 找到 `ClientAliveInterval` 设置为600到900之间
3. 重启 SSH 服务以使更改生效
   ``` shell
   systemctl restart sshd
   ```
   
# 使用加密的远程管理ssh

<b>未使用安全套接字（如SSH）加密远程管理服务器可能会导致以下危害：</b>
- 数据泄露：未加密的通信容易被截获和窃听。黑客可以捕获传输中的敏感数据，如用户名、密码、配置文件和其他重要信息。
- 中间人攻击：未加密的连接容易受到中间人攻击。攻击者可能会拦截通信，修改数据，然后将其传输到接收者，而双方可能都不知道通信已被篡改。
- 暴力破解攻击：当传输未加密的用户名和密码时，攻击者可以尝试破解这些凭据以访问服务器。如果它们使用了弱密码，攻击者可能很容易就能破解密码。
- 会话劫持：在未加密的连接中，攻击者可以劫持用户会话，然后使用已登录的凭据对服务器进行操作。
- 恶意软件和后门程序：攻击者可能会通过未加密的连接传播恶意软件或植入后门，以便在以后对服务器进行攻击。这可能会导致数据损坏、丢失或被盗。
- 泄露IP地址：未加密的连接也会泄露使用者的IP地址，这可能导致IP被跟踪，进一步增加黑客攻击发现的可能性。

1. 在sshd_config中添加或设置 `Protocol 2` 
   ``` shell
   vi /etc/ssh/sshd_config
   ```
2. 重启 SSH 服务以使更改生效
   ``` shell
   systemctl restart sshd
   ```
   
# 设置ssh登录白名单

1. 在`/etc/hosts.deny`添加 `ALL:ALL`
2. 在`/etc/hosts.allow`添加 `sshd:192.168.1.1` 
> 192.168.1.1只是一个示例  需要根据自己情况去配置


# 配置命令行超时退出
<b>未配置命令行超时退出（也称为闲置超时）可能会带来以下隐患：</b>
- 未授权访问：如果有人在用户离开计算机时尝试访问已登录的SSH会话，他们可能会有权限执行高级任务或对系统进行更改。这特别适用于具有sudo权限的用户，因为他们能够执行危险的系统操作。
- 资源占用：对于运行多个SSH会话的用户，很容易忘记关闭其中的某个。这可能导致系统资源被长期占用，从而降低服务器的整体性能。
- 难以进行故障排查：如果没有超时设置，系统管理员在分析系统日志时可能发现很多未结束的SSH会话。这会使他们难以确定哪个SSH会话是可疑的或导致问题的，从而增加排查问题的难度。

1. 在`/etc/profile`中添加`tmout=300`
   ``` shell
   vi /etc/profile
   ```
2. 使更改生效
   ``` shell
   source /etc/profile
   ```






