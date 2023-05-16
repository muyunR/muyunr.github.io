---
layout: post
title: Linux系统上安装python详细步骤
tags: Linux
image: 01.jpg
---

#### 1、默认情况下，Linux 会自带安装 Python，可以运行 python --version 命令查看

#### 2、查看 Linux 默认安装的 Python 位置

```shell
[root@VM-6-7-centos ~]# whereis python
python: /usr/bin/python2.7 /usr/bin/python3.6m /usr/bin/python /usr/bin/python3.6 /usr/lib/python2.7 /usr/lib/python3.6 /usr/lib64/python2.7 /usr/lib64/python3.6 /usr/local/lib/python3.6 /usr/include/python2.7 /usr/include/python3.6m /muyun/python3.10/bin/python3.10-config /muyun/python3.10/bin/python3.10 /usr/share/man/man1/python.1.gz
```

#### 3、安装 python3

##### 1.登录[Python Source Releases | Python.org](https://www.python.org/downloads/source/)，找到对应版本（我们以 Python 3.10.7 为例）

下载 python3.10.7.tgz

将下载号的 tgz 上传到 Linux 下的某个目录下，进行解压

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

建立软连接（Python 与 pip 都建立软连接，分别为 python3、pip3 与系统自带的 Python2 区分）

```shell
ln -s /muyun/python3.10/bin/python3.7 /usr/bin/python3
ln -s /muyun/python3.10/bin/pip3.7 /usr/bin/pip3
```

可使用命令 ls -l /usr/bin/查看软连接是否已创建成功

```shell
ls -l /usr/bin/

lrwxrwxrwx 1 root root           17 Sep 13 10:47  python3 -> /muyun/python3.10
lrwxrwxrwx 1 root root           26 Sep 13 10:48  pip3 -> /muyun/python3.10/bin/pip3

```

python，在命令窗口运行 python3

```shell
[root@VM-6-7-centos ~]# python3
Python 3.10.7 (main, Sep 14 2022, 17:45:38) [GCC 8.5.0 20210514 (Red Hat 8.5.0-4)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>

```

添加 linux 环境变量 使用 root 账号

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

#### 4、报错解决

问题 1:

```shell
configure: error: in `/root/MuYun/Python-3.10.3':
configure: error: no acceptable C compiler found in $PATHe
```

问题分析：在 ./configure --prefix=/muyun/python3.10 这一步出现这个错误，说明当前系统缺少 gcc 编译环境，
解决方法：yum -y install gcc，然后重新运行即可

问题 2:

```shell
/root/MuYun/Python-3.11.3/Modules/_ctypes/_ctypes.c:118:10: fatal error: ffi.h: No such file or directory
  118 | #include <ffi.h>
      |          ^~~~~~~
compilation terminated.

The necessary bits to build these optional modules were not found:
```

问题分析：当前系统缺少 libffi-devel
解决方法：yum install libffi-devel

问题 3:在 make 时候出现一下错误

```shell
The necessary bits to build these optional modules were not found:
_bz2                  _curses               _curses_panel
_dbm                  _gdbm                 _hashlib
_lzma                 _ssl                  _tkinter
_uuid                 nis                   readline
zlib
To find the necessary bits, look in setup.py in detect_modules() for the module's name.
```

问题分析：缺少这一堆模块，其中有一部分是本身我们不需要的，不需要去管它，但是有一些关键的模块，例如 ssl
问题解决：sudo yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel
make clean
make
