---
layout: post
title: Linux系统上卸载python
tags: Linux
image: 07.jpg
---

#### 1、查看所有 python 路劲

```shell
whereis python
```

#### 2、运行以下命令删除 python3.10 相关文件

```shell
# rm -rf /usr/bin/python3.10m
# rm -rf /usr/bin/python3.10
# rm -rf /usr/lib/python3.10
# rm -rf /usr/lib64/python3.10
# rm -rf /usr/include/python3.10m
```

#### 3、再次查看所有的 python 路径确认一下：

```shell
whereis python
```

### 还有一种卸载方法：

1、卸载 pyhton3：

```shell
rpm -qa|grep python3|xargs rpm -ev --allmatches --nodeps
```

2、删除所有残余文件

```shell
# whereis python3 |xargs rm -frv
```

3、卸载完成

4、查看现有的已安装的 python：

```shell
# whereis python
```
