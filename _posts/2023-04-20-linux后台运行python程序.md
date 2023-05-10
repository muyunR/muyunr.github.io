---
layout: post
title: linux后台运行python程序
tags: Linux
image: 05.jpg
---

## linux 后台运行 python 程序

### 一、后台运行代码命令

```shell
nohup python -u xxx.py > xxx.log 2>&1 &
```

**nohup** 加在命令的最前面，表示不挂断的运行命令

**-u** 不缓存，立即加载终端数据

**.log** 终端输出的数据，不添加将自动生成[nohup](https://so.csdn.net/so/search?q=nohup&spm=1001.2101.3001.7020).out 文件

**2>&1** 将错误内容重定向输入到标准输出中去

**&** 加载命令的最后面，表示这个命令放在后台执行

```shell
[root@trade ~]# crontab -u root -e
```

### 二、查看后台命令

**jobs** 查看当前终端后台执行的任务

**ps** 查看瞬时进程的动态，可以看到别的终端的任

### 三、结束后台任务

通过 ps 命令查看进程号 PID，然后执行 kill %PID

如果退出过客户端界面，输入 "jobs"命令查不到正在运行的程序；
输入 "ps ux"来查看所有程序的进程号 PID，然后再通过 "kill -9 PID"杀死程序；
输入 "ps ux"来查看程序是否被杀死。

如果是前台进程的话，直接执行 Ctrl+c 就可以终止了
