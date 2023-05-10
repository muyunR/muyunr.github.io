---
layout: post
title: Linus服务器后台运行python程序输出以及查看日志文件
tags: Linux
image: 03.jpg
---

## Linux 服务器后台运行 python

```shell
nohup python -u test.py > test.log 2>&1 &
```

最后的 & 表示后台运行

‘>’ 表示日志输出重定向

Linux 默认定义两个变量：2 错误输出，1 标配输出

注意：1 前面的 & 一定添加，否则回多余创建一个名为 1 的文件

## 实时查看日志文件

```shell
tail -f test.log
```

## 查看后台运行的 python 程序

```shell
ps -ef|grep python
```

## 结束指定进程

```shell
kill -s 9 进程id
```
