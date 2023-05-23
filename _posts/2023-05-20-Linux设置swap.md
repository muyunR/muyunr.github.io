---
layout: post
title: Linux设置swap
tags: Linux
image: 03.jpg
---

这些操作都在root中进行。

linux系统的swap空间类似windows系统的虚拟内存。如果你的云服务器内存小于4G，比如只有2G内存，那你就必须设置一个swap空间，否则启动服务器时会因为内存不足导致进程killed。

使用`swapon`命令可以检查系统是否已经配置过swap，云服务器一般都没有提前设置swap。

```shell
swapon -s # 如果该命令没有返回出结果，则代表该系统尚未配置过swap。
df -h	# 检查可用的存储空间
```

如果没有，按如下步骤创建Swap文件。一般建议swap大小设置为自己物理内存相同或两倍。我这里设置了4G

```shell
# 这些操作要在root中进行。
sudo fallocate -l 4G /swapfile	# 请根据自己情况修改swap大小
ls -lh /swapfile
sudo chmod 600 /swapfile	# 更改swap文件的权限，否则会有很大的安全隐患
ls -lh /swapfile	# 然后检查是否设置完成
```

启用swap文件

```shell
sudo mkswap /swapfile
sudo swapon /swapfile
# 确认一下设置是否已经生效
swapon -s
free -m
```

前面设置的swap会在重启后失效，通过修改fstab让配置永久生效

```shell
sudo vim /etc/fstab
```

在文件末尾加入下面这行内容。

```shell
/swapfile   swap    swap    sw  0   0
```
