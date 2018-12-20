##constant_score
将查询内部的结果文档得分都设定为1或者boost的值，多用于结合bool查询实现自定义得分
```
GET blog/paper/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "term": {
          "uID": "1"
        }
      }
    }
  }
}
```
bool 
布尔查询有一个或者多个布尔子句组成
|||
---|---
filter|只过滤符合条件的文档，不计算相关系得分
must|文档必须符合must中所有的条件，会影响相关性得分
must_not|文档必须不符合must_not 中的所有条件
should|文档可以符合should中的条件|
filter查询只过滤符合条件的文档，es会有只能缓存，因此其执行效率很高，做简单的匹配查询且不考虑算分是，推荐使用filter替代query
|上下文类型|执行类型|使用方式|
|--|---|--|
|Query|查找和查询语句最匹配的文档，对所有文档进行相关性算分排序|query查询 bool中的must和should|
|Filter|查找和查询语句匹配的文档|bool中的filter和must_not或者constant_score中的filter|

should 使用分两种情况
bool查询包含should,不包含must查询，只包含should,文档必须满足至少一个条件，minimum_should_match可以满足条件的个数或者百分比。
bool查询同时包含should和must查询，文档不必满足should中的条件，但是如果满足条件，会增加相关性得分。
dis_max query
function_score query
boosting query
##filter执行原理深度剖析
1.在倒排索引中查找搜索串，获取document list。
2.为每个在倒排索引中搜索到的结果，构建一个bitset，[0, 0, 0, 1, 0, 1]
3.遍历每个过滤条件对应的bitset，优先从最稀疏的开始搜索，查找满足所有条件的document
4.caching bitset，跟踪query，在最近256个query中超过一定次数的过滤条件，缓存其bitset。对于小segment（<1000，或<3%），不缓存bitset。
5.filter大部分情况下来说，在query之前执行，先尽量过滤掉尽可能多的数据
6.如果document有新增或修改，那么cached bitset会被自动更新
7.以后只要是有相同的filter条件的，会直接来使用这个过滤条件对应的cached bitset

布尔查询是一种最常用的组合查询方式，布尔查询把多个子查询组合（combine）成一个布尔表达式，所有子查询之间的逻辑关系是与（and）；只有当一个文档满足布尔查询中的所有子查询条件时，ElasticSearch引擎才认为该文档满足查询条件。布尔查询支持的子查询类型共有四种，分别是：must，should，must_not和filter：
|查询字句|说明|类型|
|--|---|--|
must|文档必须匹配must查询条件|数组
should|文档应该匹配should子句查询的一个或多个|数组
must_not|文档不能匹配该查询条件|数组
filter|过滤器，文档必须匹配该过滤条件，跟must子句的唯一区别是，filter不影响查询的score|字典
```
select * from paper where (date="2018-10-11" or uID= 1) and pID!="7ec0e0e5-a4b0-46d7-af56-5b3eab477aea"
```
```
GET blog/paper/_search
{
  "query": {
    "bool": {
      "should": [
        {"term": {"date":"2018-10-11"}},
        {"term": {"uID":1}}
      ]
      , "must_not": [
        {"term": {"pID": "7ec0e0e5-a4b0-46d7-af56-5b3eab477aea"}}
      ]
    }
  }
}
```
```
select *from paper where date= "2018-10-11" or(uid=1 and publish= 1)
```
```
GET blog/paper/_search
{
  "query": {
    "bool": {
      "should": [
        {"term": {"date": "2018-10-11"}},
        {"bool": {
          "must": [
            {"term": {"uID": "1"}},
            {"term": {"publish": true}}
          ]
        }}
      ]
    }
  }
}
```
搜索java,elasticsearch,hadoop,spark关键字需要至少匹配2个
```
GET blog/paper/_search
{
  "query": {
    "bool": {
      "should": [
        {"match": {
          "title": "java"
        }},
         {"match": {
          "title": "elasticsearch"
        }},
         {"match": {
          "title": "hadoop"
        }},
         {"match": {
          "title": "spark"
        }}
      ]
      , "minimum_should_match": 2
    }
  }
}
```
