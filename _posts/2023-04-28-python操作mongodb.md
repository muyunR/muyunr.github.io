---
layout: post
title: python操作MongoDB
tags: Python
image: 06.jpg
---

## python 操作 MongoDB

Python 要连接 MongoDB 需要 MongoDB 驱动，用 pip 安装

```shell
pip install pymongo
```

### 一、插入文档

**MongoDB**中的一个文档类似 SQL 表中的一条记录

#### **插入集合**

​ 集合中插入文档使用 **insert_one()** 方法，该方法的第一参数是字典 **name => value** 对。

例：向 **sites** 集合中插入文档

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/") #链接mongo数据库
mydb = myclient["nowcoderdb"] #创建databases对象，可以理解为进入这个库，没有则自动创建
mycol = mydb["sites"] #创建集合对象，可以理解为进入这个集合，没有则自动创建

mydict = { "name": "NOWCODER", "alexa": "10000", "url": "https://www.nowcoder.com" } #插入单个文档
mylist = [
  { "name": "Taobao", "alexa": "100", "url": "https://www.taobao.com" },
  { "name": "QQ", "alexa": "101", "url": "https://www.qq.com" },
  { "name": "Facebook", "alexa": "10", "url": "https://www.facebook.com" },
  { "name": "知乎", "alexa": "103", "url": "https://www.zhihu.com" },
  { "name": "Github", "alexa": "109", "url": "https://www.github.com" }
] #插入多个文档，以列表格式，每条数据都是字典

x = mycol.insert_one(mydict)
print(x)
```

结果如下：

```shell
<pymongo.results.InsertOneResult object at 0x10a34b288>
```

我们也可以自己指定 id，只需要在每条数据头部加入'id':value 即可

### 二、查询文档

MongoDB 中使用了 **find**和 **find_one** 方法来查询集合中的数据，它类似于 SQL 中的 SELECT 语句。

#### **查询一条数据**

​ 我们可以使用 **find_one()** 方法来查询集合中的一条数据。

例：查询 **sites** 文档中的第一条数据

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

x = mycol.find_one()
print(x)
```

结果如下：

```shell
{'_id': ObjectId('5b23696ac315325f269f28d1'), 'name': 'NOWCODER', 'alexa': '10000', 'url': 'https://www.nowcoder.com'}
```

#### **查询集合中所有数据**

**find()** 方法可以查询集合中的所有数据，类似 SQL 中的 **SELECT \*** 操作。

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

for x in mycol.find():
    print(x)
```

结果：

```json
{'_id': ObjectId('5b23696ac315325f269f28d1'), 'name': 'NOWCODER', 'alexa': '10000', 'url': 'https://www.nowcoder.com'}
{'_id': ObjectId('5b2369cac315325f3698a1cf'), 'name': 'Google', 'alexa': '1', 'url': 'https://www.google.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbb6'), 'name': 'Taobao', 'alexa': '100', 'url': 'https://www.taobao.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbb7'), 'name': 'QQ', 'alexa': '101', 'url': 'https://www.qq.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbb8'), 'name': 'Facebook', 'alexa': '10', 'url': 'https://www.facebook.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbb9'), 'name': '知乎', 'alexa': '103', 'url': 'https://www.zhihu.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbba'), 'name': 'Github', 'alexa': '109', 'url': 'https://www.github.com'}
```

#### **查询指定字段的数据**

我们可以使用 **find()** 方法来查询指定字段的数据，**将要返回的字段对应值设置为 1**。

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

for x in mycol.find({},{ "_id": 0, "name": 1, "alexa": 1 }):
    print(x)
```

结果：

```json
{'name': 'NOWCODER', 'alexa': '10000'}
{'name': 'Google', 'alexa': '1'}
{'name': 'Taobao', 'alexa': '100'}
{'name': 'QQ', 'alexa': '101'}
{'name': 'Facebook', 'alexa': '10'}
{'name': '知乎', 'alexa': '103'}
{'name': 'Github', 'alexa': '109'}
```

#### **根据指定条件查询**

我们可以在 **find()** 中设置参数来过滤数据。

例：查找 name 字段为 "NOWCODER" 的数据

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

myquery = { "name": "NOWCODER" }
mydoc = mycol.find(myquery)
for x in mydoc:
    print(x)
```

结果：

```json
{'_id': ObjectId('5b23696ac315325f269f28d1'), 'name': 'NOWCODER', 'alexa': '10000', 'url': 'https://www.nowcoder.com'}
```

#### **高级查询**

查询的条件语句中，我们还可以使用修饰符。

​ 以下实例用于读取 name 字段中第一个字母 ASCII 值大于 "H" 的数据，大于的修饰符条件为 **{"$gt": "H"}** :

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

myquery = { "name": { "$gt": "H" } }
mydoc = mycol.find(myquery)
for x in mydoc:
    print(x)
```

结果：

```json
{'_id': ObjectId('5b23696ac315325f269f28d1'), 'name': 'NOWCODER', 'alexa': '10000', 'url': 'https://www.nowcoder.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbb6'), 'name': 'Taobao', 'alexa': '100', 'url': 'https://www.taobao.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbb7'), 'name': 'QQ', 'alexa': '101', 'url': 'https://www.qq.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbb9'), 'name': '知乎', 'alexa': '103', 'url': 'https://www.zhihu.com'}
```

#### **返回指定条数记录**

​ 如果我们要对查询结果设置指定条数的记录可以使用 **limit()** 方法，该方法只接受一个数字参数。

例：返回 3 条文档记录

```python
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

myresult = mycol.find().limit(3)

# 输出结果
for x in myresult:
    print(x)
```

结果只有三条了：

```json
{'_id': ObjectId('5b23696ac315325f269f28d1'), 'name': 'NOWCODER', 'alexa': '10000', 'url': 'https://www.nowcoder.com'}
{'_id': ObjectId('5b2369cac315325f3698a1cf'), 'name': 'Google', 'alexa': '1', 'url': 'https://www.google.com'}
{'_id': ObjectId('5b236aa9c315325f5236bbb6'), 'name': 'Taobao', 'alexa': '100', 'url': 'https://www.taobao.com'}
```

### 三、修改文档

​ 我们可以在 **MongoDB** 中使用 **update_one()** 方法修改文档中的记录。该方法第一个参数为查询的条件，第二个参数为要修改的字段。如果查找到的匹配数据多余一条，则只会修改第一条。

例：将 alexa 字段的值 10000 改为 12345

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

myquery = { "alexa": "10000" }
newvalues = { "$set": { "alexa": "12345" } }

mycol.update_one(myquery, newvalues)

# 输出修改后的  "sites"  集合
for x in mycol.find():
    print(x)
```

**update_one()** 方法只能修匹配到的第一条记录，如果要修改所有匹配到的记录，可以使用 **update_many()**。

以下实例将查找所有以 **F** 开头的 **name** 字段，并将匹配到所有记录的 **alexa** 字段修改为 **123**：

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

myquery = { "name": { "$regex": "^F" } }
newvalues = { "$set": { "alexa": "123" } }

x = mycol.update_many(myquery, newvalues)
print(x.modified_count, "文档已修改")
```

### 四、排序

​ **sort()** 方法可以指定升序或降序排序。

​ **sort()** 方法第一个参数为要排序的字段，第二个字段指定排序规则，**1** 为升序，**-1** 为降序，默认为升序。

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

mydoc = mycol.find().sort("alexa")
for x in mydoc:
    print(x)
```

### 五、删除数据

我们可以使用 **delete_one()** 方法来删除一个文档，该方法第一个参数为查询对象，指定要删除哪些数据。

例：删除 name 字段值为 "Taobao" 的文档

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

myquery = { "name": "Taobao" }

mycol.delete_one(myquery)
# 删除后输出
for x in mycol.find():
    print(x)
```

#### **删除多个文档**

​ 我们可以使用 **delete_many()** 方法来删除多个文档，该方法第一个参数为查询对象，指定要删除哪些数据。

例：删除所有 name 字段中以 F 开头的文档

```python
import pymongo
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

myquery = { "name": {"$regex": "^F"} }

x = mycol.delete_many(myquery)
print(x.deleted_count, "个文档已删除")
```

#### **删除集合中的所有文档**

​ **delete_many()** 方法如果传入的是一个空的查询对象，则会删除集合中的所有文档：

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

x = mycol.delete_many({})
print(x.deleted_count, "个文档已删除")
```

#### **删除集合**

​ 我们可以使用 **drop()** 方法来删除一个集合。

以下实例删除了 customers 集合：

```PYTHON
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["nowcoderdb"]
mycol = mydb["sites"]

mycol.drop()
```

如果删除成功 drop() 返回 true，如果删除失败(集合不存在)则返回 false。
