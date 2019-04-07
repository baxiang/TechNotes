RESTful 接口 URL 的格式是：
http://cluster的地址: 9200/<index>I<type>I [<id>]
其中,index, type 是必须提供的（ index 可以理解为数据库；type 理解为数据表); id 是可选的(相当于数据库表中记 录的主键是唯一的。如果不提供， Elasticsearch 会向动生成。增 、删、改，查分别对应 HTTP 请求的 PUT 、DELETE、POST、GET方法。
##Kibana DevTools
![image.png](https://upload-images.jianshu.io/upload_images/143845-85259a9b4dc64865.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#####创建数据
创建index是province，type是citys ，当前id为1的document数据
```
PUT /province/citys/1
{
  "name":"北京",
  "code":"010"
}
```
返回结果
```
{
  "_index": "province",
  "_type": "citys",
  "_id": "1",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}
```
es自动生成id创建数据，自动生成的id 长度是20个字符，采用GUID算法，保证分布式系统，不同节点同一个时间点创建_id`生成不可能发生冲突
```
POST /province/citys/
{
  "name":"上海",
  "code":"021"
}
```
返回结果
```
{
  "_index": "province",
  "_type": "citys",
  "_id": "VV6_G2YBajYJa8_UqerL",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}
```
####修改数据
只想修改id唯一的field是name的值
```
POST /province/citys/1/_update
{
  "doc":{
    "name":"广州"
  }
}
```
删除字段
```
curl -XPOST 'localhost:9200/test/type1/1/_update' -d '{
    "script" : "ctx._source.remove(\"name_of_field\")"
}
```

#####替换数据
```
PUT /province/citys/1
{
  "name":"广州"
}
```
这个行为是替换数据，和修改数据是两种不同形式的数据更新
#####删除数据
```
DELETE /province/citys/1
```
返回结果
```
{
  "_index": "province",
  "_type": "citys",
  "_id": "1",
  "_version": 7,
  "result": "deleted",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 6,
  "_primary_term": 1
}
```
##curl命令行
 创建索引
```
$ curl -XPUT "http://127.0.0.1:9200/school"
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "school"
}
```
##删除索引
```
curl -XDELETE http://localhost:9200/school
{"acknowledged":true}
```
##插入数据
指定id 
```
$ curl -H "Content-Type:application/json"  -XPUT http://localhost:9200/school/student/1 -d '{"name":"xiaoming","age":20}'
{
    "_index": "school",
    "_type": "student",
    "_id": "1",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```
不指定id
```
curl -H "Content-Type:application/json"  -XPOST http://localhost:9200/school/student/ -d '{"name":"xiaowang","age":18}'
{
    "_index": "school",
    "_type": "student",
    "_id": "kYeii2UBT-VMLOAXEoWx",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```
##修改数据
```
curl -H "Content-Type:application/json"  -XPOST http://localhost:9200/school/student/1/_update -d '{"doc":{"name":"xiaohong","age":21}}'
{
    "_index": "school",
    "_type": "student",
    "_id": "1",
    "_version": 2,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 1,
    "_primary_term": 1
}
```
##查询数据
```
curl -XGET http://localhost:9200/school/student/1
{
    "_index": "school",
    "_type": "student",
    "_id": "1",
    "_version": 2,
    "found": true,
    "_source": {
        "name": "xiaohong",
        "age": 21
    }
}
```
## 删除
 删除数据
```
curl -XDELETE http://localhost:9200/school/student/1
{
    "_index": "school",
    "_type": "student",
    "_id": "1",
    "_version": 3,
    "result": "deleted",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 2,
    "_primary_term": 1
}
```

##删除Type
```
curl -H "Content-Type:application/json" -XPOST http://localhost:9200/school/student/_delete_by_query  -d '{"query":{"match_all":{}}}'
{
    "took": 40,
    "timed_out": false,
    "total": 1,
    "deleted": 1,
    "batches": 1,
    "version_conflicts": 0,
    "noops": 0,
    "retries": {
        "bulk": 0,
        "search": 0
    },
    "throttled_millis": 0,
    "requests_per_second": -1,
    "throttled_until_millis": 0,
    "failures": []
}
```
##bulk批量导入
```
POST _bulk
{ "index" : { "_index" : "test", "_type" : "_doc", "_id" : "1" } }
{ "field1" : "value1" }
{ "delete" : { "_index" : "test", "_type" : "_doc", "_id" : "2" } }
{ "create" : { "_index" : "test", "_type" : "_doc", "_id" : "3" } }
{ "field1" : "value3" }
{ "update" : {"_id" : "1", "_type" : "_doc", "_index" : "test"} }
{ "doc" : {"field2" : "value2"} }
```
导入结果
```
{
  "took": 176,
  "errors": false,
  "items": [
    {
      "index": {
        "_index": "test",
        "_type": "_doc",
        "_id": "1",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 0,
        "_primary_term": 1,
        "status": 201
      }
    },
    {
      "delete": {
        "_index": "test",
        "_type": "_doc",
        "_id": "2",
        "_version": 1,
        "result": "not_found",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 0,
        "_primary_term": 1,
        "status": 404
      }
    },
    {
      "create": {
        "_index": "test",
        "_type": "_doc",
        "_id": "3",
        "_version": 1,
        "result": "created",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 0,
        "_primary_term": 1,
        "status": 201
      }
    },
    {
      "update": {
        "_index": "test",
        "_type": "_doc",
        "_id": "1",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 1,
        "_primary_term": 1,
        "status": 200
      }
    }
  ]
}
```
##reindex数据迁移
```
POST _reindex
{
  "source": {
    "index": "twitter"
  },
  "dest": {
    "index": "new_twitter"
  }
}
```
##批量导入数据
```
POST /blog/paper/_bulk
{"index":{"_id":1}}
{"pID":"7ec0e0e5","uID":1,"publish":false,"date":"2018-01-02"}
{"index":{"_id":2}}
{"pID":"69590fe7","uID":2,"publish":true,"date":"2018-10-11"}
{"index":{"_id":3}}
{"pID":"aec30fa2","uID":1,"publish":false,"date":"2018-06-25"}
{"index":{"_id":4}}
{"pID":"a8733527","uID":3,"publish":false,"date":"2017-01-16"}
```
##查看es状态
查看es的健康状态
```
GET _cat/health?v
```
查看es的index
```
GET _cat/indices
```
