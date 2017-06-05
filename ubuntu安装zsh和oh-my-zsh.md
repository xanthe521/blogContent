---
title: ubuntu安装zsh和oh-my-zsh
date: 2017-03-12 12:03:32
tags:
    - ubuntu
    - zsh
    - oh-my-zsh
---



**关于系统**

我的系统是Ubuntu 14.04.5 LTS  
可以通过在终端中输入以下命令来查看当前的版本号  

```
$ cat /etc/issue  
```
输出结果类似如下  
```
Ubuntu 14.04.5 LTS \n \l
```
或者使用命令“lsb_release -a"  
```
$ sudo lsb_release -a  
```
该命令输出的内容更详细
```
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.5 LTS
Release:	14.04
Codename:	trusty
```
codename就是内部版本号  

<!--more-->

**关于bash**

```
$ cat /etc/shells
```
查看当前安装的shell
```
$ echo $SHELL
```
查看当前使用的shell

**关于 zsh 和 omz**  
最早的时候bash只有一种,但是不同的程序员有不同的需求,每个人就根据自己的需求改写了bash,最后就流传了很多种bash出来.而zsh是其中功能比较强大的一种.  

因为名字叫zsh,z是最后一个字母,所以又叫终极shell  
但是zsh得配置入门门槛很高,所以有人开发了一个zsh的扩展工具集 oh-my-zsh  
在[oh-my-zsh](http://ohmyz.sh)官网上有详细的定义

**安装 zsh 和 oh-my-zsh**  

```
$ sudo apt-get install zsh git wget
```
安装 zsh git 和 wget

```
$ wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```
获取并安装oh-my-zsh

```
$ chsh -s /bin/zsh
```
替换bash为zsh

重启terminal后就可以体验zsh和oh-my-zsh了


