---
layout: post
title: 解决Python找不到ssl模块问题 No module named _ssl
tags:  linux
image: 07.jpg
---

## python安装完毕后，提示找不到ssl模块：

```shell
>>> import ssl
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/usr/local/python27/lib/python2.7/ssl.py", line 60, in <module>
import _ssl # if we can't import it, let the error propagate
ImportError: No module named _ssl
>>>
```

### 解决方法：

1. 查看openssl安装包，发现缺少openssl-devel包

```shell
$ rpm -aq|grep openssl
    openssl-0.9.8e-20.el5
    openssl-0.9.8e-20.el5
```

2. yum安装openssl-devel

```shell
$ yum install openssl-devel -y
$ rpm -aq|grep openssl  #查看安装结果
    openssl-devel-1.0.1e-57.el6.x86_64
    openssl-1.0.1e-57.el6.x86_64
```

3. 重新编译python
   修改Setup文件

```shell
vi /src/Python-2.7.15/Modules/Setup
```

```shall
# Socket module helper for socket(2)
_socket socketmodule.c timemodule.c
# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
#SSL=/usr/local/ssl
_ssl _ssl.c \
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
-L$(SSL)/lib -lssl -lcrypto
```

4. 重新编译
   进入源码目录，重新编译安装

```shell
$ cd python-3.10.7
$ make
$ make install
```



5. 测试，已可正常使用。

```shell

$ python
>>> import ssl
>>>                # 没有报错信息代表正常
```
