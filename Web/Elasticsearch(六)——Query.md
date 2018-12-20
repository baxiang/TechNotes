查询案例的测试数据
```
POST /blog/paper/_bulk
{"index":{"_id":1}}
{"pID":"7ec0e0e5-a4b0-46d7-af56-5b3eab477aea","uID":1,"publish":false,"date":"2018-01-02","title":"this is java and elasticsearch blog"}
{"index":{"_id":2}}
{"pID":"69590fe7-3be0-4d82-94b6-ecdb661ced34","uID":2,"publish":true,"date":"2018-10-11","title":"this is java blog"}
{"index":{"_id":3}}
{"pID":"aec30fa2-10b9-4774-9569-f8d32a80267f","uID":1,"publish":false,"date":"2018-06-25","title":"this is elasticsearch blog"}
{"index":{"_id":4}}
{"pID":"a8733527-6bca-4768-ba60-253afc3d0329","uID":3,"publish":false,"date":"2017-01-16","title":"this is java,elasticsearch hadoop blog"}
```
操作简单，方便通过命令行测试，仅包含部分查询语法。常用的参数如下：
|字段|描述|
|---|---|
-q |指定查询的语句，语法Query String Syntax
-df |q 中不指定字段时默认查询字段
-sort |排序
-timeout |指定超时时间，默认不超时
-from,size|用于分页
```
GET /_index/_search?q=tom&df=user&sort=age:asc&from=4&size=10&timeout=1s
```
上面的语句意思查询userz字段包含tom的文档，结果按照age升序排列，返回第5-14个文档，如果超过1s没有结束，则超时结束
泛查询
等效于在所在字段去匹配改term
##URI Search
####指定字段查询
+代表必须包含-(减号)代表不包含
```
GET blog/paper/_search?q=title:elasticsearch
GET blog/paper/_search?q=+title:elasticsearch
GET blog/paper/_search?q=-title:elasticsearch
```
|字段|描述|
|---|---|
|took|耗费了几毫秒
|timed_out|是否超时，这里是false，代表没超时
|_shards|数据拆成了几个分片，这里是5个，所以对于搜索请求，会打到所有的primary shard（或者是他的某个replica shard也可以）
|hits.total|查询结果的数量，3个document
|hits.max_score|score的含义，就是document对于一个search的相关度的匹配分数，越相关，就越匹配，分数越高。
|hits.hits|包含了匹配搜索的document的详细数据

##full text(全文检索)
针对text类型的字段进行全文搜索，会对查询语句先进行分词处理，match,match_phrase等query类型
####match query 
es会在底层自动将math query转换成bool should语法
```
GET blog/paper/_search
{
  "query": {
    "match": {
      "title": "java elasticsearch"
    }
  }
}
```
####operator关键字
operator 控制精准度，and 表示希望分词都匹配，转换成bool must语法
```
GET blog/paper/_search
{
  "query": {
    "match": {
      "title": {
        "query": "java elasticsearch",
        "operator": "and"
      }
    }
  }
}
```
minimum_should_match关键字
```
GET blog/paper/_search
{
  "query": {
    "match": {
      "title": {
        "query": "java elasticsearch hadoop spark",
        "minimum_should_match": "75%"
      }
    }
  }
}
```
####习惯性算分
习惯性算分是指文档与查询语句间的相关度，本质是一个排序问题，排序的依据是习惯性算分。
相关系算分的重要概念
|算法|说明|
|---|---|
Term Frequery(tf)|词频，单词在该文档出现的次数，词频越高，相关度越高|
 |Document Frequery(df)|词频，单词在该文档出现的次数，词频越高，相关度越高|
 |Inverse Document Frequery(IDF)|逆向文档频率，单词出现的文档数越少，相关度越高|
 |Filed-lenght Frequery(df)|文档越短，习惯性越高|

es目前主要的两个相关性算分模型
TF/IDF
BM25模型

##exact value(精准匹配)
####match_phrase
通过slop参数控制单词间的间隔
####query_string 
类似于URL Search中的q参数查询
####simple_query_string
类似Query string 但是会忽律错误的查询语法，并且仅支持部分查询语法
####term
将查询语句作为整个单词进行查询，不会对查询语句做分词处理.

```
GET blog/paper/_search
{
  "query": {
        "term": {
          "pID": "7ec0e0e5-a4b0-46d7-af56-5b3eab477aea"
        }
  }
}
```
上面的语句是无法搜索的到的，因为通过使用分词分析，7ec0e0e5-a4b0-46d7-af56-5b3eab477aea会被分拆成4个部分建立倒排索引
```
POST /blog/_analyze
{
  "text":"7ec0e0e5-a4b0-46d7-af56-5b3eab477aea"
}
```
但是在es5.0以上版本的可以通过在filed增加keyword就可以查询到，因为text类型数据会创建两份索引，其中一份是长度为256的keyword索引数据
```
GET blog/paper/_search
{
  "query": {
        "term": {
          "pID.keyword": "7ec0e0e5-a4b0-46d7-af56-5b3eab477aea"
        }
  }
}
```
当然另外一种方式就是创建mapping,指定pID的类型是keyword,就是不分词处理，但是这个需要在我们往index 中插入数据之前，一旦插入了数据，是不能在创建mapping的，只能通过reindex重新数据迁移。
```
PUT /blog
{
  "mappings": {
    "paper": {
      "properties": {
        "pID":{
          "type": "keyword"
        }
      }
    }
  }
}

```
####terms
一次传入多个值进行搜索,类似SQL中的in查询语句
```
SELECT * FROM paper WHERE uID IN (2,3)
```
与上面的sql 查询等价的es查询语句是
```
GET blog/paper/_search
{
  "query": {
    "terms": {
      "uID": [
        "2",
        "3"
      ]
    }
  }
}

```
####match_all
获取全部数据
```
GET blog/paper/_search
{
  "query": {
    "match_all": {}
  }
}
```

####highlight search高亮搜素结果
```
GET blog/paper/_search
{
  "query": {
    "match": {
      "title": "java"
    }
  },
  "highlight": {
    "fields": {
      "title": {}
    }
  }
}
```
####Range Query
范围查询主要针对数值和日期类型
|语句|描述|
---|---
gt|greater than|
gte|greater than or equal to|
lt|less than|
lte|less than or equal to|
```
GET blog/paper/_search
{
  "query": {
    "range": {
      "date": {
        "gte": "2018-01-01",
        "lte": "2018-10-01"
      }
    }
  }
}
```
#####date math
针对日期提供的一种更加友好的计算方式now(基准日期)-1d(计算公式)
```
GET blog/paper/_search
{
  "query": {
    "range": {
      "date": {
        "gte": "2018-01-01",
        "lte": "now"
      }
    }
  }
}
```
####boost权重搜索
```
GET blog/paper/_search
{
  "query": {
    "bool": {
     
      "should": [
        {"match": {
          "title": {
            "query": "hadoop",
            "boost":3
          }
        }},
        {"match": {
          "title": {
            "query": "elasticsearch",
             "boost":2
          }
        }}
      ]
    }
  }
}
```
####sort排序
```
GET blog/paper/_search
{
  "query": {
    "match": {
      "title": "elasticsearch"
    }
  }
  , "sort": [
    {
      "date": {
        "order": "ASC"
      }
    }
  ]
}
```
