##pymogo
shell连接mongodb的命令mongo

```
$ mongo
MongoDB shell version v3.6.2
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.6.2
Server has startup warnings:
2018-08-05T14:29:13.960+0800 I CONTROL  [initandlisten]
2018-08-05T14:29:13.960+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-08-05T14:29:13.960+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-08-05T14:29:13.960+0800 I CONTROL  [initandlisten]
```
## 连接mongo
默认连接本地mongo
```
from pymongo import MongoClient
client =MongoClient()
```
参数形式连接mongo
```
client = MongoClient(host='IP', port=27017) #端口号默认为27017是数值
```
URI形式连接mongo
```
client = MongoClient('mongodb://IP:27017')
```
## 创建数据库
```
#创建数据库，两种方式
db = client.peopleinfo    
db = client['peopleinfo']  
```
连接pydb数据库
```
import pymongo

client = pymongo.MongoClient(host='localhost', port=27017)
db = client['pydb']
```
## 创建数据表
和数据库一样，下面两种方式都可以
```
t = db.stu
t = db['stu']
```
##插入数据
```
stu.insert({'_id':3,'name':'mog','age':20})
```
## 修改数据
```
stu.update({'_id':3},{'$set':{'name':'mongo'}})
```
## 查询数据
```
for cur in stu.find({'age':{'$gte':20}}):
    print(cur)
```
## 删除数据
```
stu.delete_one({'_id':'2'})
```
