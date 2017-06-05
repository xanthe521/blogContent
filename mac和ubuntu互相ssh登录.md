---
title: mac和ubuntu互相ssh登录
date: 2017-03-12 20:14:24
tags:
     - ssh
---

### 背景

家里局域网内有一台mac和一台电脑安装的ubuntu,  
现在需要经常互相ssh登录,   
所以希望配置一下达到两个目的  

1. 免密码登录

2. 登录不输入ip地址  

为了方便演示,将mac系统的ip地址定为192.168.1.2   
ubuntu系统的IP地址定为192.168.1.3  
用户名都为aaa  

<!--more-->
#### 安装ssh  
mac系统是自带ssh的,但是需要在设置中打开远程登录来使用  
ubuntu需要安装openssh-server  

```
$ sudo apt-get install openssh-server
```
安装完成后就可以在mac系统中登录ubuntu系统了
```
$ ssh aaa@192.168.1.3
```
第一次链接会提示你确认指纹,直接输入yes然后输入ubuntu系统的aaa用户密码后就能链接成功  

在ubuntu系统上同理也可以登录mac系统了  

#### 配置免密码登录

在链接过程中需要输入密码确认,这是比较繁琐的,  
我们可以通过rsa免密码登录方法免除输密码的繁琐  
首先为mac系统创建公钥私钥对  

```
$ ssh-keygen -t rsa -C "注释"
```

输入后会提示一下信息,一般连续按三次空格后就完成公钥私钥对的创建  

```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/aaa/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/aaa/.ssh/id_rsa.
Your public key has been saved in /Users/aaa/.ssh/id_rsa.pub.
The key fingerprint is:
......
```

完成后在 ~/.ssh 文件夹中会生成 id_rsa 和 id_rsa.pub 两个文件  
id_rsa.pub 文件里的内容就是公钥  
复制公钥内容后登陆 ubuntu 系统,将公钥内容复制到 ~/.ssh/authorized_keys 中  
现在从 mac 系统中就可以直接免密码登陆 ubuntu 系统了  
接下来为 ubuntu 系统创建公钥私钥对,创建方法和mac系统一致   
不过将公钥内容复制到mac系统中就比较方便了

```
$ ssh-copy-id -i ~/.ssh/id_rsa.pub aaa@192.168.1.2
```
会提示你输入192.168.1.2的aaa用户密码,输入即可  
现在从 ubuntu 上登陆 mac 系统就不用输入密码了

#### 配置别名  

配置别名在 mac 系统和 ubuntu 系统上都是一样的  
在 .ssh 文件夹的config文件中添加配置信息  
如果没有config文件通过命令创建  
```
$ cd ~/.ssh
$ touch config
```
配置信息实例  
```
Host myMac
HostName 192.168.1.2
User aaa
Port 22
IdentitiesOnly yes
```
Port 是端口号,不写默认为22.IdentitiesOnly 表示只接受 SSH key 登录  
在 ubuntu 系统中的 ~/.ssh/config 中这么配置后,终端中输入
```
$ ssh myMac
```
即可登录 mac 系统,mac系统中也是一样.
