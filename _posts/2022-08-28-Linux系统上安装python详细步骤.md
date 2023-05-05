---
layout: post
title:  Linux系统上安装python详细步骤
tags:  Linux,python
image: 01.jpg
---

#### 1、默认情况下，Linux会自带安装Python，可以运行python --version命令查看

#### 2、查看Linux默认安装的Python位置

```shell
[root@VM-6-7-centos ~]# whereis python
python: /usr/bin/python2.7 /usr/bin/python3.6m /usr/bin/python /usr/bin/python3.6 /usr/lib/python2.7 /usr/lib/python3.6 /usr/lib64/python2.7 /usr/lib64/python3.6 /usr/local/lib/python3.6 /usr/include/python2.7 /usr/include/python3.6m /muyun/python3.10/bin/python3.10-config /muyun/python3.10/bin/python3.10 /usr/share/man/man1/python.1.gz
```

#### 3、安装python3

##### 1.登录[Python Source Releases | Python.org](https://www.python.org/downloads/source/)，找到对应版本（我们以Python 3.10.7为例）

下载python3.10.7.tgz

将下载号的tgz上传到Linux下的某个目录下，进行解压

```shell
tar -xf /muyun/Python-3.10.7.tgz
```

进入解压后的目录并且执行

```shell
cd /muyun/Python-3.10.7
./configure --prefix=/muyun/python3.10  # 指定安装目录为/muyun/python3.10
```

编译

```shell
make		
```

编译安装

```shell
make install
```

建立软连接（Python与pip都建立软连接，分别为python3、pip3与系统自带的Python2区分）

```shell
ln -s /muyun/python3.10/bin/python3.7 /usr/bin/python3
ln -s /muyun/python3.10/bin/pip3.7 /usr/bin/pip3
```

可使用命令ls -l /usr/bin/查看软连接是否已创建成功

```shell
ls -l /usr/bin/

lrwxrwxrwx 1 root root           17 Sep 13 10:47  python3 -> /muyun/python3.10
lrwxrwxrwx 1 root root           26 Sep 13 10:48  pip3 -> /muyun/python3.10/bin/pip3

```

python，在命令窗口运行python3

```shell
[root@VM-6-7-centos ~]# python3
Python 3.10.7 (main, Sep 14 2022, 17:45:38) [GCC 8.5.0 20210514 (Red Hat 8.5.0-4)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 

```

添加linux环境变量 使用root账号

```shell
vi /etc/profile
```

在末尾添加

```shell
#python
PATH=/muyun/python3.10/bin:$PATH
```

保存后 执行

```shell
source /etc/profile
```

安装完成
