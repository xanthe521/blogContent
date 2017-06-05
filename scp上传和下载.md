---
title: scp上传和下载
date: 2017-03-18 00:18:03
tags:
    - ssh
    - scp
---




scp的英文全称是"secure copy" ,可以通过scp命令在linux系统之间传送文件,mac系统也支持这一命令,而且传输是加密的.最近用的比较多,总结一下:

1. 在ssh文件中设置的config文件中,如果有设置了别名,scp命令中也是支持使用别名的

2. scp可以将文件,文件夹从本地传到目的主机,或者从目的主机传到本地  

<!--more-->  

### 从本地到目的主机  
假设 ~ 目录下有 a.txt 和 foo 文件夹,目的主机地址已经在config中设置别名为server1  
传送到server1中的 ~/recive 文件夹  

文件  


```
$ scp ~/a.txt server1:~/recive/a.txt
```
注: server1 中~/recive 目录需要存在, a.txt 文件不需要存在

文件夹
```
$ scp -r ~/foo server1:~/recive
```
-r 表示递归复制,复制该文件夹下的文件和目录



### 从目的主机到本地
假设 目的主机地址已经在config中设置别名为server1  ,需要将 server1 中 ~/foo 文件夹 和 a.txt传送到本地的 ~/recive 文件夹中

文件
```
$ scp server1:~/a.txt ~/recive/a.txt
```

文件夹
```
$ scp -r server1:~/foo ~/recive/foo
```

### 可能有用的参数  

-P 端口;  
-p 表示保持文件权限;  
-r 表示递归复制;  
-v 和大多数 linux 命令中的 -v 意思一样，用来显示进度，可以用来查看连接、认证或是配置错误；
-C 使能压缩选项；  
-4 强行使用 IPV4 地址；  
-6 强行使用 IPV6 地址；  


