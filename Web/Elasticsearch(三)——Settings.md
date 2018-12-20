##设置索引的分片
7.0之前默认会创建5个shared,7.0开始之后创建一个shard
```
#! Deprecation: the default number of shards will change from [5] to [1] in 7.0.0; if you wish to continue using the default of [5] shards, you must manage this on the create index request or with an index template
```
##获取设置
```
$curl -XGET http://localhost:9200/school/_settings
{
    "school": {
        "settings": {
            "index": {
                "creation_date": "1535647751974",
                "number_of_shards": "5",
                "number_of_replicas": "5",
                "uuid": "EruOYFQ9TNSpmiZb_PdPXw",
                "version": {
                    "created": "6020499"
                },
                "provided_name": "school"
            }
        }
    }
}
```
## 修改分片
```
PUT /my_temp_index/_settings
{
    "number_of_replicas": 1
}
```

## 创建设置和映射
|说明|实例|
|---|---|
|类型|PUT|
|URL| http://localhost:9200/school|
```
{
	"settings":{"number_of_replicas":5},
	"mapping":{"student":{"properties":{"name":{"type":"text","index":"true"}}}}
}
```
## 别名
##索引分析
```
http://localhost:9200/_analyze
{
	"analyzer":"standard",
	"text":"welcome to CHINA"
}
```
```
{
    "tokens": [
        {
            "token": "welcome",
            "start_offset": 0,
            "end_offset": 7,
            "type": "<ALPHANUM>",
            "position": 0
        },
        {
            "token": "to",
            "start_offset": 8,
            "end_offset": 10,
            "type": "<ALPHANUM>",
            "position": 1
        },
        {
            "token": "china",
            "start_offset": 11,
            "end_offset": 16,
            "type": "<ALPHANUM>",
            "position": 2
        }
    ]
}
```
## 重建索引_reindex
```
curl -H "Content-Type:application/json" -XPOST http://localhost:9200/_reindex -d'{"source":{"index":"school"},"dest":{"index":"school_v2"}}'
{
    "took": 254,
    "timed_out": false,
    "total": 1,
    "updated": 0,
    "created": 1,
    "deleted": 0,
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

