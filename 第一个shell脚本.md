---
title: 第一个shell脚本
date: 2017-03-19 15:40:56
tags:
    - shell
---


  最近才接触类unix系统,接触了一些sh脚本  
  正好有个项目运行的时候需要启动3个 memcached 进程  
  在windows系统下很简单,直接操作窗口打开关闭就行
  mac上的memcached是运行在终端中的,需要通过命令行来启动和关闭  
  要关闭的时候每次都是先去查到所有的 pid ,然后在手动填入 kill 命令中
  看起来就很麻烦,所以尝试用sh脚本来试试  
<!--more-->

  #### 启动
  创建一个 memcached.sh 文件  
  向文件中写入启动3个 memcached 进程的命令
  ```  
  $ memcached -m 10M -p 11211 -d
  $ memcached -m 10M -p 11212 -d
  $ memcached -m 10M -p 11213 -d  
  ```

  最开始的时候因为 memcached 有参数 -P <file> 可以将进程 pid 存入文件  
  但是实践后发现这么执行后只会有最后一个进程的 pid  
  所以干脆就不用 memcached 的参数,自己来保存
  ```
  $ pgrep memcached > pid.log
  ```
  pgrep 用来输出模糊查询的进程 pid  
  相比于 ps -ef | grep 命令,这个命令会忽略掉 grep 查询的进程  
  注:如果想在ps -ef | grep 命令的时候也忽略掉 grep 查询的进程,可以这么写  
  ```
  $ ps -ef | grep memcached | grep -v 'grep'
  ```

  这样在命令行执行 ./memcached.sh 后会启动3个 memcached 进程,并将进程 pid 保存到 pid.log 文件中  

  #### 停止
  正常的停止的命令应该是  
  ```
  $ kill -9 pid1 pid2 pid3
  ```
  不过启动的时候已经将 pid 存到 pid.log 文件中了,所以可以读出文件中的值作为 kill 命令的参数
  ```
  $ cat pid.log | xargs kill -9
  ```
  xargs 的作用是将前面管道传来的值作为后面命令的参数  

  #### 合并
  最初的意思是启动用一个脚本,停止用一个脚本.不过后来想这样很麻烦,完全可以写在一起  
  脚本中 $1 的意思表示执行脚本时传入的第一个参数,$2 $3  同理  
  下面是完整的脚本  
  ```

  if [[ $1 = 'start' ]]; then
    #statements
    memcached -m 10M -p 11211 -d
    memcached -m 10M -p 11212 -d
    memcached -m 10M -p 11213 -d
    pgrep memcached > pid.log
  elif [[ $1 = 'stop' ]]; then
    #statements
    cat pid.log | xargs kill -9
    rm -rf pid.log
  else
    echo "need start or stop argument"
  fi

```
启动或者停止的时候在后面加上 start 或者 stop 参数就可以了
