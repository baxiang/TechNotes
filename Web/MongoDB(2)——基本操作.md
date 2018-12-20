## 数据库
查看当前数据库名称,默认的数据库为test
```
>db
test
```
列出所有在物理上存在的数据库
```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```
切换数据库,则指向数据库，但不创建，直到插入数据或创建集合时数据库才被创建
```
use pydb
switched to db pydb
```
删除当前指向的数据库,如果数据库不存在，则什么也不做
```
>db.dropDatabase()
{ "ok" : 1 }
```
stats()查看当前数据库信息
```
> db.stats()
{
	"db" : "pydb",
	"collections" : 1,
	"views" : 0,
	"objects" : 3,
	"avgObjSize" : 47,
	"dataSize" : 141,
	"storageSize" : 36864,
	"numExtents" : 0,
	"indexes" : 1,
	"indexSize" : 36864,
	"fsUsedSize" : 161156169728,
	"fsTotalSize" : 249779191808,
	"ok" : 1
}
```
##集合
创建集合
```
> db.createCollection("student")
{ "ok" : 1 }
```

查看当前数据库集合
```
 show collections
student
student1
```
删除集合db.集合名称.drop()
```
>db.student1.drop()
true
```
## 插入
db.集合名称.insert(document)
```
> db.student.insert({name:'bx',age:25})
WriteResult({ "nInserted" : 1 })
> db.student.insert({_id:'2',name:'bx1',age:18})
WriteResult({ "nInserted" : 1 })
```
文档的_id已经存在则修改，如果文档的_id不存在则添加
```
db.集合名称.save(document)
```
增加一条数据和insert相同
```
db.student.save({name:'testname',age:23})
WriteResult({ "nInserted" : 1 })
> db.student.find()
{ "_id" : ObjectId("5b69c1cf9776b7d034e8a3dd"), "name" : "bx", "age" : 25 }
{ "_id" : "2", "name" : "bx2", "age" : 20 }
{ "_id" : ObjectId("5b69c4959776b7d034e8a3de"), "name" : "testname", "age" : 23 }
```
修改数据
object id每个文档都有一个属性，为_id，保证每个文档的唯一性，可以自己去设置_id插入文档
如果没有提供，那么MongoDB为每个文档提供了一个独特的_id，类型为objectID，objectID是一个12字节的十六进制数，前4个字节为当前时间戳，接下来3个字节的机器ID，接下来的2个字节中MongoDB的服务进程id，最后3个字节是简单的增量值。
```
db.student.save({_id:ObjectId("5b69c4959776b7d034e8a3de"),name:'test',age:16})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
## 删除
```
db.集合名称.remove(
   <query>,
   {
     justOne: <boolean>
   }
)
```
参数query:可选，删除的文档的条件。
参数justOne:可选，默认false，表示删除多条,true或1，则只删除一条。
```
db.student.remove({name:'test'},{justOne:true})
WriteResult({ "nRemoved" : 1 })
```
##修改
```
db.集合名称.update(
   <query>,
   <update>,
   {multi: <boolean>}
)
```
参数query:查询条件，类似sql语句update中where部分
参数update:更新操作符，类似sql语句update中set部分
参数multi:可选，默认false，只修改第一条记录，true表示满足条件的文档全部修改
```
> db.student.update({_id:'2'},{name:'bx2',age:20})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
## 查看
```
> db.student.find()
{ "_id" : ObjectId("5b69c1cf9776b7d034e8a3dd"), "name" : "bx", "age" : 25 }
{ "_id" : "2", "name" : "bx1", "age" : 18 }
```
条件查询
```
db.student.find({name:'bx'})
{ "_id" : ObjectId("5b69c1cf9776b7d034e8a3dd"), "name" : "bx", "age" : 25 }
```
条件查询返回需要显示的字段，_id默认是显示 不需要显示的需要设置成0
```
> db.student.find({name:'bx'},{age:1,_id:0})
{ "age" : 25 }
```
显示集合的个数
```
> db.student.find().count()
3
```
